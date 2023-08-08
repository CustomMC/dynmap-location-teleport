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
/* Version: 2.0.0 */
/* Last update: 2023/08/08 */

var mcdev = {
    configs: {
        selector: document,
        listener: "mouseup",
        fixedY: 250
    },
    events: {
        onMouseUp: function (e) {
            let utils = mcdev.utils;
            switch (e.which) {
                case 1:
                    let location = e.ctrlKey && e.altKey ? utils.getLocation(true) : (e.ctrlKey ? utils.getLocation(false) : null);
                    if (location != null) { return utils.copyToClipboard(utils.commandByCoord(location)); }
                    break;
            }
            return false;
        }
    },
    utils: {
        commandByCoord: function (coord) { return "/tp " + coord.replaceAll(",", " "); },
        copyToClipboard: function (text) {
            try {
                navigator.clipboard.writeText(text).then(() => {
                    console.log(`Copied text to clipboard: ${text}`);
                    return true;
                }).catch((error) => { console.error(`Could not copy text: ${error}`); });
            } catch (error) { alert(`Navigator Clipboard not found: ${error}`); }
            return false;
        },
        getLocation: function (array) {
            array = array || false;
            let coord = $(".coord-control-value").html();
            if (array) {
                arrayCoord = coord.split(",");
                return arrayCoord[0] + " " + mcdev.configs.fixedY + " " + arrayCoord[2];
            }
            return coord;
        }
    }
}
// Initialize on Document Ready
$(document).ready(function () { $(mcdev.configs.selector).on(mcdev.configs.listener, function (e) { mcdev.events.onMouseUp(e); }); });
```

## Usage
### `CTRL` + `Left Mouse Button`
Generate a tp command to the mouseHover location as described by Dynmap locator on top-left of your screen.

**WARNING!** The Y value is 64, we may suggest to use this teleport while in Spectator Mode.

### `CTRL` + `ALT` + `Left Mouse Button`
Generate a tp command to the mouseHover location as described by Dynmap locator on top-left of your screen.

The Y, in this case, will be the configured fixedHeight value. Plese check the configuration area to know more.

## Configuration
- `selector` used to define the selector to generate the event on mouse click (Default `document`);
- `listener` define the mouse click event, you can even change with your own eventType of JQuery (Default `mouseup`);
- `fixedY` define the fixed "Y" coord when typing `CTRL` + `ALT` + `Left Mouse Button`. (Default `250` to prevent spawing inside a block).

## Local development
Don't forget... `navigator.clipboard` is accessible only from secure connections (domain with a valid ssl).

If you want to test it locally please enable `Insecure origins treated as secure` adding your Local IP or Local Domain in the Chrome Textarea (`--unsafely-treat-insecure-origin-as-secure=myexamplesite.loocal`) from `chrome://flags`.

Also, you can even check if your domain is secure using the bool `window.isSecureContext` from JavaScript.
