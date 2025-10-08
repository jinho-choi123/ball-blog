# Ball Blog

This is my personal blog.

## Prerequisites for serving in local

1. Install `zola`

```bash
brew install zola
```

2. Setup python environment for pre-commit

```bash
uv sync
pre-commit install
```

3. Serve the blog

```bash
zola serve
```

## How to write a post

All the blog contents are located in `content/` directory.
It consists a tree structure like this:

```text
content/
    - Home/
    - Computer-System/
    - AI-Accelerator/
    - Programming/
    - ...
```

### Generating a index.md file

For example, to write a post about "address sanitizer" in `Computer-System` section, you can create a "address-sanitizer" directory in `content/Computer-System/` directory.
And you can write a post in `address-sanitizer/index.md` file.

```text
content/
    - Computer-System/
        - address-sanitizer/
            - index.md
```

Also the index.md file should have a title, slug, and date.

```text
+++
title = "Address Sanitizer"
slug = "address-sanitizer"
date = 2025-01-01
+++
```

### Inserting a image

You can insert a image by using `img` tag. The images should be located in the same directory as the index.md file.(This is just a convention.)

```text
<img src="image.png" alt="image">
```

### Inserting a KaTex Equation

You can insert a [KaTex](https://katex.org/) equation by wrapping the equation with `$$` or `$`.

```text
$$
\int_0^1 x^2 dx = \frac{1}{3}
$$
```
