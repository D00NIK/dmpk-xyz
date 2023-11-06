# simple-hugo
A simple [Hugo](https://gohugo.io/) theme for you to use.

## Demo
[My personal site](https://dominikpakula.xyz/) uses it. Find source code [here](https://github.com/D00NIK/dominikpakula-xyz/).

## Usage
```bash
hugo new site my-site
git clone https://github.com/D00NIK/simple-hugo.git my-site/themes/simple-hugo
echo "theme = 'simple-hugo'" >> my-site/hugo.toml # change for other formats
```

## Configuration (`hugo.toml`)

### Website title
Just change the `title`.
```toml
title = 'My New Hugo Site'
```

### Top right navigation
Add it with Menus like below. Weight determines the order.

```toml
[menu]
    [[menu.main]]
        name = 'Blog'
        pageRef = '/blog'
        weight = 1

    [[menu.main]]
        name = 'Contact'
        pageRef = '/contact'
        weight = 2
```

### Footer socials
Same as above but with `footer` menu. Do **not** erase the name, it is required by Hugo. Don't forget the logo in `static` folder.

```toml
[menu]
    [[menu.footer]]
        name = 'RSS'
        url = 'https://dominikpakula.xyz/index.xml'
        weight = 2
        [[menu.footer.params]]
            path = 'static/logos/rss.svg'

    [[menu.footer]]
        name = 'Github'
        url = 'https://github.com/D00NIK/'
        weight = 1
        [[menu.footer.params]]
          path = 'static/logos/github.svg'
```

### Footer text
Add it as in example below. It works with HTML too.

```toml
[params]
    footerText = "Built with <a href='http://gohugo.io' target='_blank' rel='noopener noreferrer'>Hugo</a>"
```

## Extra
This theme is [Unlicensed](https://github.com/D00NIK/simple-hugo/blob/master/LICENSE). Attribution is appreciated but not required.

There isn't much options to edit. This means that it probably won't fit your needs. And that's okay. Just change the theme to *your* needs!
