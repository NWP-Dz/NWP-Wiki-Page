```markdown
# ONM-NWP Wiki Page

Welcome to the ONM-NWP Wiki Page repository! This guide will help you understand how to modify and update the content of our wiki page effectively.

## Table of Contents
- [Repository Structure](#repository-structure)
- [Modifying Content](#modifying-content)
- [Building the Documentation](#building-the-documentation)
- [Deploying Changes](#deploying-changes)
- [Additional Resources](#additional-resources)

## Repository Structure

The repository is structured as follows:

```
your-repo-name/
│
├── docs/
│   ├── index.md                # Home page
│   ├── Overview/
│   │   ├── Content.md           # Overview content
│   │   ├── FileFormats.md       # File formats documentation
│   │   └── Source.md            # Source information
│   ├── ForecastModel/
│   │   ├── Atmospheric/
│   │   │   ├── ALADIN.md        # ALADIN atmospheric model documentation
│   │   │   ├── AROME.md         # AROME atmospheric model documentation
│   │   │   └── AROME_Dust.md    # AROME dust model documentation
│   │   └── Marine/
│   │       └── ww3.md          # WaveWatch III model documentation
│   └── ...
├── mkdocs.yml                  # Configuration file for MkDocs
└── README.md                   # This file
```

## Modifying Content

1. **Clone the Repository**:
   If you haven't already, clone the repository to your local machine:
   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   ```

2. **Navigate to the Docs Directory**:
   Change into the `docs` directory where all Markdown files are stored:
   ```bash
   cd your-repo-name/docs
   ```

3. **Edit Markdown Files**:
   Open any `.md` file you wish to modify using your preferred text editor. For example, to edit the `Content.md` file:
   ```bash
   nano Overview/Content.md
   ```
   or
   ```bash
   code Overview/Content.md
   ```

4. **Save Your Changes**:
   After making your changes, save the file.

## Building the Documentation

To view your changes locally, you need to build the documentation. First, ensure you have MkDocs installed. If you haven't installed it yet, do so using pip:

```bash
pip install mkdocs
```

Now, to build and serve the documentation locally, run:

```bash
mkdocs serve
```

This command will start a local server, and you can view the wiki page by visiting `http://127.0.0.1:8000` in your web browser.

## Deploying Changes

1. **Stage and Commit Your Changes**:
   Once you're satisfied with your modifications, you need to stage and commit your changes:
   ```bash
   git add .
   git commit -m "Your commit message describing the changes"
   ```

2. **Push Changes to GitHub**:
   Push your changes to the main branch:
   ```bash
   git push origin master
   ```

3. **Deploy to GitHub Pages**:
   After pushing, deploy the changes to GitHub Pages:
   ```bash
   mkdocs gh-deploy
   ```

This command will update the `gh-pages` branch with the latest HTML files based on your Markdown changes. Your updates will be live on the wiki page shortly after deployment.

## Additional Resources

- [MkDocs Documentation](https://www.mkdocs.org/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)

Feel free to reach out if you have any questions or need assistance!
```

### Instructions for Use

- **Replace placeholders**: Ensure to replace `your-username` and `your-repo-name` in the links with your actual GitHub username and repository name.
- **Customize content**: Feel free to add any additional instructions or details specific to your project that you think would be useful for your colleagues.
