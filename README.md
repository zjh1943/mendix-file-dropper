


[![Apache License](https://img.shields.io/badge/license-Apache%202.0-orange.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Support](https://img.shields.io/badge/Support-Community%20(no%20active%20support)-orange.svg)](https://docs.mendix.com/developerportal/app-store/app-store-content-support)
[![Studio](https://img.shields.io/badge/Studio%20version-8.0%2B-blue.svg)](https://appstore.home.mendix.com/link/modeler/)
![GitHub release](https://img.shields.io/github/release/mendixlabs/mendix-file-dropper)
![GitHub issues](https://img.shields.io/github/issues/mendixlabs/mendix-file-dropper)
[![DeepScan grade](https://deepscan.io/api/teams/7221/projects/9412/branches/122227/badge/grade.svg)](https://deepscan.io/dashboard#view=project&tid=7221&pid=9412&bid=122227)
[![Available](https://img.shields.io/badge/Test%20Project-available-green.svg)](https://github.com/mendixlabs/widget-test-projects)


# Fork to add features:

I fork this repository to change the file uploadindg time, from when selecting a file, to when submitting the form.  
Before change, when a user select a file, the file will be uploading to the server imediately. This will make the user feel bad when he/she uploads the wrong file or wants to undo the uploading before submit. A normal user usually thinks the file should be upload only after clicking the submit button. But the default behavior of this widget is not that.
After change, you can make the widget upload the file when clicking the submit button.  
This can be done by set the following properties:

![Property window of form linkage](./assets/property_window_form_linkage.png)

If you set the `Save file on form submit` as `false`, the behavior will remain same as the origin widget.
If you set the `Save file on form submit` as `true`, you will get the behavior described above.

# FileDropper

Inspired by the [Mendix Dropzone widget](https://appstore.home.mendix.com/link/app/916/). Drop files/images in your Mendix application. WebModeler compatible! This widget is based on [react-dropzone](https://github.com/react-dropzone/react-dropzone) and [MobX](https://github.com/mobxjs/mobx) (version 4, needed for IE11 compatibility).

![appstore](/assets/AppStoreIcon.png)

Show this dropzone:

![preview](/assets/screenshot.png)

## Features

- Drop files in a dropzone on your page
- Automatically upload to Mendix
- Save using a `POST` method (enabled progress bar) or `saveDocument` (this uses the `mx.data.saveDocument` method and should work offline)
- Restrict files based on size, number of files and mime types
- Verify a file after it is uploaded (onAccept Mf), or use a Verification Entity (see below) and verify before using a microflow or nanoflow
- Execute microflow/nanoflow after it is succesfully uploaded
- Show/hide labels & image previews

> Widget size: ~180Kb, which is ~52Kb Gzipped online
> Test project can be downloaded [here](https://github.com/JelteMX/widget-test-projects#file-dropper)

## Compatibility

### Mendix version

Only works in Mendix 8.0.0 and upwards. This widget was created using MobX, which needs a newer React version. Due to that limitation this widget will not work in Mendix versions lower than 8.

### Browsers

- IE 11
- Chrome,Firefox,Safari,Edge
- Should work on Mobile Web, but untested. If you find an issue, please let me know!

**Known issues:**

- In IE11, preview might not work or become very slow. If you experience problems with IE 11 (who uses this anyway??), please switch of previews.
- Progress bar relies on SVG which does not render properly in Microsoft Edge. It's on the Todo to replace this with a normal DIV element

## Basic configuration

### 1. Data

![configuration1](/assets/configuration1.png)

- Select and Entity that is (or extends) a `System.FileDocument` (`System.Image` also works)
- **Name** attribute is required, others are not
- Auto save is by default set to true. If for whatever reason you want to give the user control over that, set this to false
- Save Method:

We include the normal **POST** method (default) which will do a request to the Mendix server. The upside of that is that it includes a progress bar, which is beneficial if you handle big files or have a slow connection. If for whatever reason this doesn't work, you can use the **saveDocument** method. This uses `mx.data.saveDocument` and should (need to verify) also work offline.

### 2. Restrictions

![configuration2](/assets/configuration2.png)

- Max file size can be set to 0 to have no restrictions. This is always in Mb (1024 * 1024 bytes)
- Max files can be set to 0 to have no restrictions. If it is set to 1, the dropzone (which is clickable) will also restrict the amount of files you can select in the file dialog
- Mime types is used to restrict the type of files that can be uploaded. This is not fool-proof and might not work the same across browsers. Furthermore, if the Entity set in Data is of type `System.Image`, this option will be ignored.

### 3. Verification

![configuration3a](/assets/configuration3a.png)
![configuration3b](/assets/configuration3b.png)

Verification can be done in two ways:

- onAccept Mf: Microflow that is executed after upload. This can be done when handling small files, but will polute your system with files that are uploaded and maybe not removed
- beforeAccept Mf/Nf: This method uses an extra Entity (non-persistant!) that will be filled with some data from the file (name, size, type, extension) and sent to a microflow/nanoflow. This Mf/Nf will return a string. If the string is `""` or `empty`, it is considered accepted. If the string contains a text, this is used as the error message in the widget.

### 4. Events

- Before commit:
  - Microflow/Nanoflow that is executed before committing new files. It can be used to test out context object, to see if we need to commit it first (when associating the files with our context)
- After commit:
  - This microflow/nanoflow (can be used both) will be executed after a succesful upload. It will send the file object itself as an input parameter

### 5. UI

![configuration5](/assets/configuration5.png)

- For various icons you can either use the standard Bootstrap Gylphicon (the classname will be prefixed with `glyphicon glyphicon-`) or a built-in icon.
- You can switch the preview off entirely
- You can switch the type label, previews off
- You can switch off the progress bar once it has been uploaded.
- You can switch off the filesize info
- For the progress bar you can set different colors, depending on the status

### 6. Texts

![configuration6](/assets/configuration6.png)

- Various texts can be configured. These are actually translatable strings, so the can be translated in Mendix based on the locale.
- You can switch off the Confirmation dialog by leaving the Delete text empty

## Demo project

> Test project can be downloaded [here](https://github.com/mendixlabs/widget-test-projects#file-dropper)

Here is the used Domain Model:

![domainmodel](/assets/domain-model.png)

## Issues, suggestions and feature requests

Please file an issue here on Github

## Development and contribution

TBD...

## TODO

- Add unit tests
- Add e2e tests
- Replace progress bar with proper div so it works in Edge
- Add styling properties (class prefixes etc), replace some BEM

## IDEAS

- This widget is perfect right? Who needs new ideas? ;-)

## License

Apache 2
