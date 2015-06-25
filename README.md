# GAPS (Google APps Script)
>The easiest way to develop Google Apps Script projects from the command line.

## Requirements
  - [node v0.12.x](https://nodejs.org/download/)
  - `npm install -g node-google-apps-script`

## Quickstart

### 1. Create and Save a Blank Apps Script Project

1. Head to [https://script.google.com](https://script.google.com) and create a new blank project.
2. Save the mostly empty project and give it a name.
  - Saving is important; the project is not in your Google Drive until it is saved.
3. Add the Underscore Library to your project.
  1. Click `Resources` > `Libraries`
  3. Paste `MGwgKN2Th03tJ5OdmlzB8KPxhMjh3Sh48` (Underscore) into the `Find a Library` box and select it.
  4. Select the most recent version of the library from the `Version` dropdown.
  5. Click the `Save` button.

### 2. Get Google Drive Credentials

You can do this one of two ways:

1. Adding Google Drive permissions to the default Developer Console Project that
  is created for each Apps Script project
2. Using an independent Developer Console Project and enabling the Google Drive
  API

#### 2.1 Exising Apps-Script-Created Dev Console Project

1. Access the automatically created apps script Developer Console Project by
  1. `Resources` > `Developers Console Project`
  2. Click on the blue link at the top (`project-name - api-project-##########`)
    to access the correct Developer Console Project
2. Enable the Google Drive API
  1. Click `APIs & auth` in the left nav and then select `APIs`
  2. Search for `Drive` and select the Google Drive API listing.
  3. Click `Enable API`
3. Acquire Google Drive Client Secret Credentials
  1. In the `Credentials` section of the `APIs & auth` group, Select
    `Create New Client ID`
  2. In the menu that appears, choose `Installed application` for the `Application type`.
    - You can leave the `Installed Application Type` field as `Other`.
  3. Click `Create Client ID`.
  4. Finally, download your credentials using the `Download as JSON` button below them.
    - Save these credentials to a location of your choosing; `~/Downloads` is fine.
  5. You may close the Developer Console window.

#### 2.2 Independent Dev Console Project

1. Use [this link](https://console.developers.google.com/start/api?id=drive&credential=client_key) to create the project.
  - That link will auto-activate the Google Drive API.
  - If you have multiple Google Accounts, append `&authuser=1` to the end of the url to choose which account to login with.
    - note that `authuser` is 0-indexed.
3. Make sure `Create a New Project` is selected and hit `Continue`.
4. Once the project has been created, click `Go to Credentials`.
5. Select `Create New Client ID`, choose the `Installed Application` type, and then click `Configure Consent Screen`.
6. Select your email address from the dropdown and assign your add-on a Project Name.
  - This can always be changed later.
7. `Save` your Consent Screen
8. In the credentials section (which you should be redirected to after the last step),
   Select `Create New Client ID`
9. In the menu that appears, choose `Installed application` for the `Application type`.
  - You can leave the `Installed Application Type` field as `Other`.
10. Click `Create Client ID`.
11. Finally, download your credentials using the `Download as JSON` button below them.
  - Save these credentials to a location of your choosing; `~/Downloads` is fine.
12. You may close the Developer Console window.
  - To return to this project later, select `Resources` > `Developer Console Project`
    while editing your script. Then click the link at the top of the dialog to open the Developer Console with this project selected.

### 3. Authenticate node-google-apps-script

1. Run `gaps auth ~/Downloads/path/to/client_secret_abcd.json`
  - **i.e.** `gaps auth ~/Downloads/client_secret_1234567890-abcd.apps.googleusercontent.com.json`
  - This process will set up Google Drive authentication to allow uploading and importing of the Apps Script project.
2. Follow the directions by clicking on the link generated by the script.

### 4. Initialize your project

1. Get your project ID from the address bar, located after `/d/` and before `/edit`.
  - It should look something like `1RRks8iFLH1K9RN58Rt5RHo4-OEnUUDlPn-nN_cz4EcxK82eo2NsLb_ba`
2. Run `gaps init <projectId> [subdir]` (see Usage below for details)
  - This will generate `.manifest.json` with a representation of the state of
  your Apps Script project on the Apps Script servers.
3. Awesome! Now, to upload changes, run `gaps upload`. You should then be able
   to reload your script and see changes.
  - To see changes to your script files,
    [https://script.google.com](https://script.google.com) and select your script from the recent projects.

### 5. Upload New Files

1. `gaps upload [subdir]` will upload files to Google Drive from an (optional)
subdirectory.

## Best Practices

- Only upload to Google Drive and never edit files in the web IDE
  - This way the true state of the code is locally or in git and you'll never
    need to deal with issues where the states locally and remotely are not in
    sync.
  - You also get the benefits of structuring code within directories
- Add `.manifest.json` to `.gitignore`
  - There is no need to push it to GitHub as the manifest is only needed locally
    for gaps to work correctly.
- Don't use `gaps download`
  - To use other's code, clone the files from git and upload them.

## Usage

### Initialize a local project with an Apps Script Project

Run `gaps init <projectId> [subdir]` to link your local files to that
Apps Script project. This creates a minimal `.manifest.json` containing the
state of the Apps Script project on the server.

The optional `[subdir]` parameter allows you to specific a subdirectory for your
local files. `gaps init <projectId> src` for example.

### Upload a project
Run `gaps upload [subdir]` (alias: push) from the root of your project's
directory to push it back up Google Drive. You'll have to refresh your browser
to view changes.

Any files deleted locally will also be deleted in Drive, and any files created
locally will be added to Drive. Google Apps Script only supports .js and .html
files right now. Make sure any new script files are .js (not .gs) when created
locally.

The optional `[subdir]` parameter allows you to specify a subdirectory to upload
instead of the root directory. `gaps upload src`, for example.

### Sync a project

If you're attempting to upload, but getting cryptic 400 errors
(see `Common Issues` below), replace your local `.manifest.json` with the
server's by calling `gaps sync`. Then you will be able to upload
changes, overwriting the files on the server.

### Download a project
**It is not recommended to use `gaps download`; see `Development Workflow and Usage` for best practices.**

Run `gaps download <projectId> [subdir]` to download an existing Google Apps Script
project.

This command will create an exact copy of your project on your local file system
and store it in `[subdir]` or your current working directory (just like git
clone).

**This command will overwrite existing files with the same name as your project.**


### Version control
The inspiration for this project came from Google's lack of version control.
By having a local copy of your script, you can easily track files in a git repo
and manage them with GitHub.

## Development
Please submit any bugs to the Issues page. Pull Requests also welcome.

If you want to develop, clone down the repo and have at it! You can run
`npm link` from the root directory of the repo to symlink to your local copy.
You'll have to uninstall the production version first
`npm uninstall -g node-google-apps-script`.

## Limitations

**apps-script-starter-kit** allows you to nest files in folders, but the
Apps Script platform expects a flat file structure. Because of this,
**no files can have the same name, even if they are in separate directories**.
One file will overwrite the other, making debugging difficult.

Your add-on must be developed as a
[standalone script](https://developers.google.com/apps-script/guides/standalone)
and tested within Doc or Sheet. This means that it cannot use certain functions
of [bound scripts](https://developers.google.com/apps-script/guides/bound),
namely installable triggers. While testing within a Doc, you have access to the
"Special methods" mentioned in
[the docs](https://developers.google.com/apps-script/guides/bound), though. If
you followed the quickstart above, you should be set up correctly.

## Common Issues

The Google Drive API frequently returns a *400* error without a helpful error
message. Common causes for this are:

- **javascript formatting errors**
  - The server-side javascript code (`.js` or `.gs`) is validated upon upload.
  - If there is a syntax error, the upload will fail.
- The project does not exist yet.
  - Verify that the project exists in your Google Drive folder.
- Missing server-side libraries
  - verify that any required Libraries (notably `Underscore`) have been added
    to the Apps Script project
- Authentication error
  - Verify that `~/.gaps` exists and has 4 values: `client_id`, `client_secret,
    `redirect_uri`, and `refresh_token`
  - To reset authentication, delete `~/.gaps` and then perform the
    authentication steps in part #2 of the quickstart again.

## Extra Resources

- [Google Apps Script Documentation](https://developers.google.com/apps-script/)
