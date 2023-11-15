---
title: "My biggest problem with Prisma: Error Handling"
date: "2023-11-15"
---

Don't get me wrong, I really love it, it's very intuitive and easy to use. But there aren't any *good* alternatives (if I'm wrong, **please** correct me) and I end up using it every time I work with a database in Next.js.

## To the point, what's the problem?

Imagine a situation where you're building an API in Next.js, not just for your own use, but you want to list it publicly. You get some payload in JSON, and you want to make sure everything goes into the database flawlessly. That's not a hard thing to do, Prisma will throw an error if you try to save incorrect data, but when building an API, you also want to inform a user what's wrong. And here we are, simply wanting clear information about an error. In prisma every error is well documented, if not, we can just test it out and see which one it throws. And then we access the message, which looks something like this:

```bash
Invalid `prisma.user.create()` invocation:

{
  data: {
    name: 123,
          ~
    password: "coolpassword123"
  }
}

Argument `name`: Invalid value provided. Expected String, provided Int.
```

It does look okay in the terminal, but if you try to return it as JSON, it will have all these \n chars. Of course there isn't anything we can work with, and I just had to slice the last part, from "Argument" until the end.

But sadly, it's not enough. Sometimes the types mess it up, for example:

```bash
"Argument `name`: Invalid value provided. Expected String or StringFieldUpdateOperationsInput, provided Int."
```

What's left? Just to remove the expected part, I guess, or do some fancy operations to include the first two and last two words in that sentence.

## Of course, it is not everything.

Another example is error code `P2025`. It will come out whenever you try to search for something, but it was simply not found. What's fascinating about this one is that it is being thrown in multiple ways.

If you try to simply return what the user is asking for, it will return this:

```bash
{
  name: 'NotFoundError',
  message: 'No User found.',
  code: 'P2025',
  clientVersion: '5.5.2',
  meta: undefined
}
```

But trying to update a record and it being not found returns that:

```bash
{
  name: 'PrismaClientKnownRequestError',
  code: 'P2025',
  clientVersion: '5.5.2',
  message: "\nInvalid `prisma.user.update()` invocation:\n\n\nAn operation failed because it depends on one or more records that were required but not found. Record to update not found.",
  meta: { cause: 'Record to update not found.' }
}
```

Do you see this inconsistency? Now I should return `meta.cause` since it is much clearer for the user. I ended up with this solution:

```js
case "P2025":
      return Response.json({ message: `${err.meta?.cause || err.message}` }, { status: 404 });
```

Of course, this one only works for this specific error, so you must test out every possible one with trial and error.

## Conclusion

Annoying. Of course, you should test your API first, but having to add more errors as an App grows is just a total pain in the ass. This just makes me want to leave my APIs closed or not create them in Next.js at all. Please leave a like [here](https://github.com/prisma/prisma/issues/5040) so developers can make it a reality (or do it yourself if you're ambitious). The best for me would be adding a short comment on what has gone wrong, so I wouldn't even have to create an own function for all prisma-related errors (which felt weird at first, but as I said, I don't think there's a better solution).

So that's about it. Thanks for reading. If you have any suggestions, or I'm just too stupid to do it easier, please contact me.