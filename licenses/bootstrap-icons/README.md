# Bootstrap Icons

- Version: 1.13.1
- Upstream: https://github.com/twbs/icons/tree/v1.13.1
- License: MIT (see LICENSE)
- Source archive: https://github.com/twbs/icons/archive/refs/tags/v1.13.1.tar.gz

Selected SVGs are stored as `assets/icons/bi-*.svg`. Each SVG has
`aria-hidden="true" focusable="false"` added because it decorates a visible text label.
The original paths, viewBox, and currentColor behavior are retained.

To update, download an explicit release, replace the selected SVGs and LICENSE,
retain the accessibility attributes, update this version, and build the site.
Use a `bi-` prefix to avoid collisions with theme icons. Add icons in menus with
`pre = "bi-house"` or templates with `{{ partial "icon.html" "bi-house" }}`.
