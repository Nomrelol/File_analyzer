# File Analyzer & Treemap Visualizer

This is a powerful, zero-installation, client-side-only file browser and analysis tool packed into a single HTML file. It allows you to load a local folder, browse its contents, analyze file types, preview files, and visualize your storage usage with an interactive treemap.

Because it runs entirely in your browser, **no files are ever uploaded to a server**, ensuring your data remains completely private.



---

## 🚀 Core Features

This tool mimics several common command-line utilities (`ls`, `cat`, `wc`, `zip`) in a rich graphical interface.

* **Folder Browsing (`ls`):** Load an entire local folder (using `webkitdirectory`) to list all nested files.
* **Interactive Dashboard:** Get an instant overview of your folder, including:
    * **Total Files:** A count of all files loaded.
    * **Total Size:** The combined size of all files.
    * **Largest File:** The name of the single largest file.
    * **Newest File:** The name of the most recently modified file.
* **File Type Analysis:** An interactive doughnut chart (powered by **Chart.js**) shows the distribution of file types by extension (e.g., `.js`, `.css`, `.png`).
* **Storage Treemap (`treemap`):**
    * A key visualization feature using **D3.js** rendered on an HTML Canvas.
    * Each file is represented as a rectangle, where its area is proportional to its file size.
    * Files are grouped and colored by their parent directory.
    * **Interactive Tooltip:** Hover over any rectangle to see the file's full path, name, and exact size.
* **File Preview (`cat`):**
    * Click any file in the list to preview its content directly in the inspector.
    * Supports text-based files (e.g., `.txt`, `.js`, `.json`, `.md`).
    * Supports common image formats (e.g., `.png`, `.jpg`, `.gif`).
    * Large files (>2MB text, >5MB image) are truncated or disabled to ensure browser performance.
* **File Properties & Word Count (`wc`):**
    * When a file is selected, the inspector shows its full **properties**: Name, Size, Type, and Last Modified date.
    * For any text-based file, the inspector automatically calculates and displays its **word count**: Lines, Words, and Characters.
* **Filtering & Sorting (`sort`):**
    * Instantly filter the file list by name (e.g., `.jpg`, `readme`).
    * Sort the list by **Name** (A-Z, Z-A), **Size** (Smallest, Largest), or **Type** (A-Z).
    * A "Quick Select" dropdown allows you to jump to and preview any file from the filtered list.
* **Batch Selection & ZIP (`zip`):**
    * Select multiple files using checkboxes.
    * Use **Select All** (`Ctrl+A`) / **Clear Selection** to manage large selections.
    * Download all selected files as a single `.zip` archive, created on-the-fly in your browser using **JSZip**.
* **Hotkeys for Power Users:**
    * **`Ctrl+F`**: Focus the filter input.
    * **`Ctrl+A`**: Select all files in the current (filtered) list.
    * **`Ctrl+S`**: Download the currently selected files as a ZIP (note: this may conflict with the browser's "Save As" hotkey).

---

## 🛠️ How to Use

1.  **Download:** Save the provided code as an `index.html` file on your computer.
2.  **Open:** Open the `index.html` file in a modern web browser (e.g., Google Chrome, Microsoft Edge, Firefox).
3.  **Load Folder:** Click the **"Choose Folder"** button (in the toolbar or the pinned footer).
4.  **Grant Permission:** Your browser will ask for permission to read the contents of the folder you selected. You must **allow** this for the tool to work.
5.  **Analyze:** Once loaded, the dashboard, file list, and treemap will instantly populate.
6.  **Explore:**
    * Click a file in the list to preview it.
    * Use the filter and sort options to find files.
    * Check the boxes next to files you want to download.
    * Click **"Download Selected"** to get your `.zip` file.
    * Click and drag on the treemap canvas to explore file sizes visually.

---

## 🔧 Technical Deep Dive

This application is intentionally built as a single file to be self-contained and portable. It relies on a few key browser APIs and external (CDN-hosted) libraries.

### Libraries Used
* **Tailwind CSS:** For the entire responsive, dark-mode UI.
* **JSZip:** For client-side ZIP archive generation.
* **Chart.js:** For the file-type distribution doughnut chart in the dashboard.
* **D3.js (v7):** Used for all treemap calculations (`d3.hierarchy`, `d3.treemap`) and color scaling (`d3.scaleOrdinal`).

### Core JavaScript Structure

The application's logic is contained within a single `window.App` object to avoid polluting the global namespace. This "Few-Variable Hero" strategy organizes the code into three main parts:

* `App.state`: Holds all dynamic data, such as `allFiles`, the `fileHierarchy` (for the treemap), and the `selectedFiles` Set.
* `App.dom`: Caches references to all necessary DOM elements for fast access.
* `App.methods`: Contains all application logic, such as `handleFileSelect`, `renderFileList`, `renderTreemap`, and `handleZipDownload`.

### Key Browser APIs

* **File System API (`webkitdirectory`):** The non-standard `webkitdirectory` attribute on the `<input type="file">` element is what enables folder selection. This is supported by most modern browsers.
* **FileReader API:** Used to read the contents of local files for the preview (`cat`) and word count (`wc`) features.
* **Canvas API:** The D3.js treemap is rendered onto a `<canvas>` element (rather than SVG) for better performance when visualizing thousands of files.

---

## ⚠️ Limitations

* **Browser Support:** This tool's core folder-reading functionality relies on `webkitdirectory`. It is fully supported in Chromium-based browsers (Chrome, Edge) and Firefox. Safari has partial support, which may behave differently.
* **Memory Usage:** Loading a folder with hundreds of thousands of files may cause the browser to become slow or unresponsive, as all file metadata is stored in memory.
* **File Previews:** Previews for very large files (>5MB images, >2MB text) are intentionally disabled or truncated to prevent the browser tab from crashing.
* **Read-Only:** This is a file *analyzer*, not an editor. It can only read your files. The only "write" operation is downloading a new ZIP file to your "Downloads" folder. It **cannot** modify or delete your original files.