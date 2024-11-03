For instructions on how to install and use - see https://www.globus.org/globus-connect-personal.

To configure the accessible directories in Globus Connect Personal, follow these steps based on your operating system:

**For Windows:**

1. **Access Options:**
   - Right-click the Globus icon in the system tray (bottom-right corner).
   - Select "Options..." from the context menu.

2. **Modify Accessible Folders:**
   - In the "Access" tab, you'll see a list of directories currently accessible.
   - To add a new directory:
     - Click the "+" icon.
     - Navigate to and select the desired folder.
     - Ensure the "Writable" checkbox is checked if you want write access.
   - To remove an existing directory:
     - Select it from the list.
     - Click the "-" icon.

3. **Save Changes:**
   - Click "Save" to apply your new settings.

**For macOS:**

1. **Access Preferences:**
   - Click the Globus icon in the menu bar at the top of your screen.
   - Select "Preferences..." from the dropdown menu.

2. **Adjust Accessible Directories:**
   - Navigate to the "Access" tab.
   - To add a new directory:
     - Click the "+" icon.
     - Browse to and select your desired folder.
     - Check the "Writable" box if you need write permissions.
   - To remove an existing directory:
     - Highlight it in the list.
     - Click the "-" icon.

3. **Apply Changes:**
   - Click "Save" to confirm your modifications.

**For Linux:**

1. **Edit Configuration File:**
   - Open the configuration file located at `~/.globusonline/lta/config-paths` in a text editor.
   - Each line in this file specifies an accessible directory in the format:
     ```
     /path/to/directory,sharing_flag,read_write_flag
     ```
     - `/path/to/directory`: Absolute path to the directory.
     - `sharing_flag`: Set to `1` to allow sharing, `0` to disallow.
     - `read_write_flag`: Set to `1` for read/write access, `0` for read-only.

2. **Modify Accessible Paths:**
   - To set a new root path, add a line with your desired directory. For example:
     ```
     /desired/root/path,0,1
     ```
     This grants read/write access without sharing permissions.
   - Remove or comment out the line corresponding to your home directory if you no longer want it accessible.

3. **Restart Globus Connect Personal:**
   - After saving your changes, restart GCP to apply them:
     ```bash
     ./globusconnectpersonal -stop
     ./globusconnectpersonal -start &
     ```

By following these steps, you can customize the directories that Globus Connect Personal accesses on your system.
