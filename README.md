# Dynmap JavaScript Extension (Location Teleport)
You can now automatically generate a `/tp` command on your mouseOn location by clicking "CTRL + Left Mouse Button".

## Installation
1. Go to your server plugin folder `<yourServerLocation>/plugins/dynmap/web/js`
2. Open the file `dynmaputils.js`
3. Copy and paste the following Script on bottom of the file content:
```JavaScript
/* Location Teleport */
/* You can now automatically get a /tp command to mousehover location to your Clipboard */
/* Author: Marco Cusano */
/* Version: 1.0.0 */
/* Last update: 2023/08/07 */

$(document).ready(function () {
    $(document).on('mouseup', function (e) {
        switch (e.which) {
            case 1:
                if (e.ctrlKey) {
                    let coord = $(".coord-control-value").html();
                    let command = "/tp " + coord.replaceAll(",", " ");
                    return copyToClipboard(command);
                }
                break;
        }
        return false;
    });
});

function copyToClipboard(text) {
    try {
        navigator.clipboard.writeText(text).then(() => {
            console.log(`Copied text to clipboard: ${text}`);
            return true;
        }).catch((error) => { console.error(`Could not copy text: ${error}`); });
    } catch (error) { alert(`Navigator Clipboard not found: ${error}`); }
    return false;
}
```

## Usage
### `CTRL` + `Mouse Left Button`
Generate a tp command to the current location as described by Dynmap locator on top-rleft of you screen.
**WARNING!** The Y value is 64, we may suggest to use this teleport while in Spectator Mode.

