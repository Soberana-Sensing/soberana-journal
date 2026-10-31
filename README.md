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

## License

All content in this repository is licensed under the
[Creative Commons Attribution 4.0 International](LICENSE) license (CC BY 4.0).
You are free to share and adapt the material, including commercially, as long as
you credit Soberana Sensing and link the license, and indicate if changes were
made.
