# <i class="fas fa-tools"></i> MkDocs Setup Guide

## <i class="fas fa-list-check"></i> Prerequisites

- <i class="fab fa-python"></i> Python 3.x installed on your system
- <i class="fas fa-download"></i> pip (Python package installer)

## <i class="fas fa-download"></i> Installation Steps

### 1. <i class="fas fa-box"></i> Install MkDocs and Material theme

**On Windows:**
```bash
py -m pip install mkdocs mkdocs-material
```

**On Linux/Mac:**
```bash
pip install mkdocs mkdocs-material
```

### 2. <i class="fas fa-folder-open"></i> Navigate to the MkDocs directory

```bash
cd 02_portfolio/0.1_mkdocs
```

### 3. <i class="fas fa-server"></i> Start the MkDocs development server

**On Windows:**
```bash
py -m mkdocs serve
```

**On Linux/Mac:**
```bash
mkdocs serve
```

### 4. <i class="fas fa-globe"></i> Open your browser

Navigate to: `http://127.0.0.1:8000`

## <i class="fas fa-link"></i> Useful MkDocs Links

### <i class="fas fa-book"></i> Official Documentation
- <i class="fas fa-home"></i> [MkDocs Documentation](https://www.mkdocs.org/) - Official MkDocs documentation
- <i class="fas fa-book-open"></i> [MkDocs User Guide](https://www.mkdocs.org/user-guide/) - Complete user guide
- <i class="fas fa-cog"></i> [MkDocs Configuration](https://www.mkdocs.org/user-guide/configuration/) - Configuration options

### <i class="fas fa-paint-brush"></i> Material Theme
- <i class="fas fa-palette"></i> [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) - Material theme documentation
- <i class="fas fa-rocket"></i> [Material Theme Getting Started](https://squidfunk.github.io/mkdocs-material/getting-started/) - Quick start guide
- <i class="fas fa-bookmark"></i> [Material Theme Reference](https://squidfunk.github.io/mkdocs-material/reference/) - Theme reference

### <i class="fab fa-github"></i> GitHub Repositories
- <i class="fab fa-github"></i> [MkDocs on GitHub](https://github.com/mkdocs/mkdocs) - MkDocs source code
- <i class="fab fa-github"></i> [Material for MkDocs on GitHub](https://github.com/squidfunk/mkdocs-material) - Material theme source code

### <i class="fas fa-puzzle-piece"></i> Additional Resources
- <i class="fas fa-plug"></i> [MkDocs Plugins](https://github.com/mkdocs/mkdocs/wiki/MkDocs-Plugins) - Available plugins
- <i class="fas fa-theater-masks"></i> [MkDocs Themes](https://github.com/mkdocs/mkdocs/wiki/MkDocs-Themes) - Available themes
- <i class="fas fa-users"></i> [MkDocs Community](https://github.com/mkdocs/mkdocs/discussions) - Community discussions

### <i class="fas fa-quick"></i> Quick Reference
- <i class="fas fa-terminal"></i> [MkDocs Commands](https://www.mkdocs.org/user-guide/cli/) - Command-line reference
- <i class="fas fa-pen"></i> [Writing Your Docs](https://www.mkdocs.org/user-guide/writing-your-docs/) - Writing documentation
- <i class="fas fa-cloud-upload-alt"></i> [Deploying Your Docs](https://www.mkdocs.org/user-guide/deploying-your-docs/) - Deployment options

## <i class="fas fa-wrench"></i> Troubleshooting

### <i class="fas fa-sync-alt"></i> Changes Not Showing on Page Refresh

If your latest changes are not appearing when you refresh the page, try these solutions:

#### 1. <i class="fas fa-redo"></i> Hard Refresh Your Browser
- **Windows/Linux:** Press `Ctrl + Shift + R` or `Ctrl + F5`
- **Mac:** Press `Cmd + Shift + R`
- This clears the browser cache and forces a fresh load

#### 2. <i class="fas fa-server"></i> Check if MkDocs Server is Running
- Make sure the server is running in your terminal
- You should see output like: `INFO - Serving on http://127.0.0.1:8000/`
- If not running, start it with: `py -m mkdocs serve` (Windows) or `mkdocs serve` (Linux/Mac)

#### 3. <i class="fas fa-power-off"></i> Restart the MkDocs Server
- Stop the server (press `Ctrl + C` in the terminal)
- Start it again: `py -m mkdocs serve` (Windows) or `mkdocs serve` (Linux/Mac)
- This ensures all changes are picked up

#### 4. <i class="fas fa-save"></i> Verify File is Saved
- Make sure you've saved the markdown file (`Ctrl + S`)
- Check that the file is in the correct location: `docs/index.md` or `docs/setup.md`

#### 5. <i class="fas fa-exclamation-triangle"></i> Check Terminal for Errors
- Look for any error messages in the terminal where MkDocs is running
- Common issues: YAML syntax errors, missing files, or path issues

#### 6. <i class="fas fa-trash-alt"></i> Clear Browser Cache Completely
- Open browser developer tools (`F12`)
- Right-click the refresh button and select "Empty Cache and Hard Reload"

**Note:** MkDocs should automatically reload when files change, but sometimes a hard refresh or server restart is needed.

### <i class="fas fa-exclamation-circle"></i> Common Issues

#### <i class="fab fa-python"></i> Python/pip Not Found
If you get an error that `pip` is not recognized:
- **Windows:** Use `py -m pip` instead of `pip`
- **Linux/Mac:** Make sure Python and pip are installed and in your PATH

#### <i class="fas fa-network-wired"></i> Port Already in Use
If port 8000 is already in use:
```bash
py -m mkdocs serve -a 127.0.0.1:8001
```
This will start the server on port 8001 instead.

#### <i class="fas fa-file-code"></i> YAML Configuration Errors
If you see YAML errors:
- Check `mkdocs.yml` for syntax errors
- Make sure indentation is correct (use spaces, not tabs)
- Verify all file paths in the `nav` section are correct

#### <i class="fas fa-box-open"></i> Missing Dependencies
If MkDocs doesn't start:
- Reinstall dependencies: `py -m pip install --upgrade mkdocs mkdocs-material`
- Check Python version: `py --version` (should be 3.x)

### <i class="fas fa-question-circle"></i> Getting Help

If you're still experiencing issues:
1. <i class="fas fa-book"></i> Check the [MkDocs documentation](https://www.mkdocs.org/)
2. <i class="fab fa-github"></i> Search [MkDocs GitHub issues](https://github.com/mkdocs/mkdocs/issues)
3. <i class="fas fa-comments"></i> Ask in the [MkDocs discussions](https://github.com/mkdocs/mkdocs/discussions)

