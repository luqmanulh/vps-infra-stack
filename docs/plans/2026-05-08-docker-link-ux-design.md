# Docker Link UX Design

## Objective
Improve the User Experience (UX) of the `README.md` by instructing readers to open the Docker installation guide in a new tab, preventing them from losing their place in the deployment guide.

## Motivation
Markdown does not natively support `target="_blank"` for hyperlinks. While embedding raw HTML `<a>` tags solves the web-rendering issue, it degrades the readability of the raw text document in terminal environments.

## Approach
Maintain 100% pure Markdown formatting. We will append a concise instructional text next to the Docker installation link: `*(Ctrl+Click or Middle-Click to open in a new tab)*`. This guides the user behavior directly without compromising document cleanliness.
