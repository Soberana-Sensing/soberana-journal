# Soberana Sensing Journal

The source of the journal posts published at
[soberana-sensing.org/journal](https://soberana-sensing.org/journal). The website
fetches this repository's `main` branch at build time, so a merged pull request
automatically appears on the site on the next deploy.

## Structure

One folder per post. The folder name is the URL slug (`/journal/<folder>`).

```
<slug>/
  post.md    # frontmatter + markdown (the post)
  images/    # assets referenced by the post
  RESULTS/   # optional: data and figures produced by the analysis
```

`post.md` starts with frontmatter (`published`, `title`, `excerpt`, `tags`) and
the rest is standard markdown: headings, lists, tables, fenced code blocks, and
images referenced by their path relative to the post folder.

## Contributing

Anyone is welcome to propose edits or new posts through a pull request. Direct
pushes to `main` are blocked; all changes go through PRs so they can be reviewed
before publication.

To propose a change:

1. Create a new branch for your contribution (if you cannot push branches to
   this repository, fork it first and create the branch on your fork).
2. Make your edit: for a new post, create a folder named after your post's URL
   slug and add a `post.md` inside it; for an edit, modify the existing
   `post.md`.
3. Commit and push your branch, then open a pull request against `main` and
   describe what you changed and why.
4. A reviewer will look at your PR. Once it is merged, the change appears on
   the website at the next deploy.

## License

All content in this repository is licensed under the
[Creative Commons Attribution 4.0 International](LICENSE) license (CC BY 4.0).
You are free to share and adapt the material, including commercially, as long as
you credit Soberana Sensing and link the license, and indicate if changes were
made.
