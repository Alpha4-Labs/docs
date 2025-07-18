# Contributing to Alpha Points Documentation

Thank you for contributing! This repo (https://github.com/Alpha4-Labs/docs) centralizes documentation for the Alpha Points protocol, including architecture diagrams, module overviews, and guides.

## Guidelines
- **Standards**: Use Markdown for all files. Follow UML 2.5 for diagrams (Mermaid syntax; see [GitHub Mermaid docs](https://docs.github.com/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams#creating-mermaid-diagrams)). Prefer concise, engineering-spec compliant content (e.g., avoid wordy diagrams).
- **File Structure**: Place architecture in /architecture/, features in /features/, smart contract overviews in /modules/, frontend docs in /frontend/.
- **Content Rules**: Include code snippets with citations (e.g., ```1:10:sources/partner.move). Trace symbols to definitions. Cite sources (e.g., PRs like [PR #123](https://github.com/Alpha4-Labs/alpha-points/pull/123)).

## Pull Request Process
1. Fork the repo and create a branch (e.g., feature/new-diagram).
2. Make changes, commit with descriptive messages (e.g., 'Add sequence diagram for PartnerCap creation').
3. Open a PR against main with a clear title/description, referencing related issues.
4. Ensure changes render correctly (e.g., test Mermaid in a GitHub issue).
5. Await review; address feedback.

## Tools and Best Practices
- **Editing**: Use VS Code or Cursor for Markdown/Mermaid previews.
- **Testing**: Validate diagrams via Mermaid Live Editor. Run linters if applicable.
- **Issues**: Report bugs or suggest features via GitHub Issues.
- **License**: Contributions are under MIT (see LICENSE.md once added).

For questions, open an issue or contact maintainers. 