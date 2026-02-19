# beheader (Windows Port)  
Polyglot generator for media files.  
  
This is a Windows‑compatible port of the original Linux‑only project:    
[https://github.com/p2r3/beheader](https://github.com/p2r3/beheader)  
  
The goal of this fork is to make beheader run natively on Windows systems by adapting Linux‑specific dependencies, paths, and command behavior.  
  
## Windows Port Notes  
This fork modifies the original script to work on Windows.    
Key changes include:  
  
- Using Git for Windows to provide `unzip.exe` and `zip.exe`  
- Replacing Linux `/tmp` paths with Windows temporary directories  
- Adjusting ImageMagick calls (`magick convert` instead of `convert`)  
- Ensuring `mp4edit.exe` (Bento4) is available for Windows  
- Handling Windows path separators (`\` vs `/`)  
  
## Dependencies (Windows)  
You must install the following tools and ensure they are available in your `PATH`:  
  
- Bun JavaScript runtime — https://bun.sh  
- FFmpeg and FFprobe — Windows builds available [here](https://www.ffmpeg.org/download.html)  
- ImageMagick — use `magick convert` instead of `convert`  
- Bento4 mp4edit.exe — https://www.bento4.com  
- Git for Windows — provides `unzip.exe` and `zip.exe`  
  
Default Git for Windows paths:  
1. `C:\Program Files\Git\usr\bin\unzip.exe`  
2. `C:\Program Files\Git\usr\bin\zip.exe`  
  
These replace the Linux `zip` and `unzip` utilities.  
  
## Dependencies (Linux — Original Project)  
If you're on Linux, the original README applies:  
  
- Bun  
- ffmpeg  
- ffprobe  
- ImageMagick `convert`  
- zip  
- unzip  
- mp4edit  
  
If you use Nix or NixOS, the included `flake.nix` will install everything automatically.  
  
## Usage  
With all dependencies set up, you should be able to run:  
`bun run beheader.js <output> <image> <videoaudio> [-options] [appendable...]`
  
### Positional arguments  
- `output` — Path of resulting polyglot file.  
- `image` — Path of input image file.  
- `video|audio` — Path of input video (or audio) file.  
- `appendable` — Path(s) of files to append without parsing.  
  
### Optional flags  
- `-h`, `--html` `<path>` — Path to HTML document.  
- `-p`, `--pdf` `<path>` — Path to PDF document.  
- `-z`, `--zip` `<path>` — Path to ZIP‑like archive. Can be repeated.  
- `-e`, `--extra` `<path>` — Path to short (<200 byte) file to include near the header.  
- `--help` — Print usage guide and exit.  
  
## Technical Notes  
1. The merging process is not necessarily lossless.    
   Video or audio is re‑encoded to MP4, images are converted to PNG (inside an ICO container), HTML is bundled with a stylesheet, PDF offsets are adjusted, and ZIP archives are repacked.  
  
2. Many file formats use ZIP internally (JAR, APK, PPTX, DOCX, XLSX, etc.).    
   ZIPs are appended after extras.  
  
3. The `--extra` data is inserted at address 22.    
   Exceeding ~200 bytes may break other components.  
  
## Output Behavior  
The resulting polyglot file changes behavior depending on its extension:  
  
- `.ico` — displays the input image  
- `.mp4` — plays the input video  
- `.html` — shows the input webpage  
- `.pdf` — opens the input PDF (if applicable)  
- `.zip` — extracts the input archive (if applicable)  
  
Some programs may reject the file due to unusual metadata or structure.  
