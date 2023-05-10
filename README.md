# Troubleshooting

### Windows

![system-detected-an-overrun-of-a-stack-based-buffer](/windows/system-detected-an-overrun-of-a-stack-based-buffer.jpg "system-detected-an-overrun-of-a-stack-based-buffer")

1. Run SFC/DISM

   - Type cmd, and select Run as administrator under Command Prompt.

     ```
     DISM /online /cleanup-image /scanhealth
     ```

     ```
     DISM /online /cleanup-image /restorehealth
     ```

     ```
     sfc /scannow
     ```

2. Microsoft Safety Scanner
