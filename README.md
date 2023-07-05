Ulysses supports [x-callback URLs](http://x-callback-url.com/), allowing other apps to trigger certain actions in Ulysses such as opening existing sheets or creating new sheets, groups or attachments.

# Reference

## The URL Scheme
The used scheme is `ulysses://`. Every action can be called through the x-callback-url API using the following format:

	ulysses://x-callback-url/[action]?[x-callback parameters]&[parameters]

For more information regarding the URL format, see the [official specification](http://x-callback-url.com/specifications/).

## Available Actions
In the following, all available actions are detailed. Please note that for reasons of clarity, the examples are not yet URL encoded. For instance, all whitespace must be replaced with `%20`.

* [new-sheet](#new-sheet)
* [new-group](#new-group)
* [insert](#insert)
* [attach-note](#attach-note)
* [update-note](#update-note)
* [remove-note](#remove-note)
* [attach-image](#attach-image)
* [attach-keywords](#attach-keywords)
* [remove-keywords](#remove-keywords)
* [set-group-title](#set-group-title)
* [set-sheet-title](#set-sheet-title)
* [move](#move)
* [copy](#copy)
* [trash](#trash)
* [get-item](#get-item)
* [get-root-items](#get-root-items)
* [read-sheet](#read-sheet)
* [get-quick-look-url](#get-quick-look-url)
* [open](#open)
* [open-all](#open-any), [open-recent](#open-any), [open-favorites](#open-any)

<a id="new-sheet"></a>

### new-sheet

Creates a new sheet.

<span class="availability-version">*Available on iOS since Ulysses 2.6 ([API version 1](#api-versions)), on Mac since Ulysses 2.8 ([API version 2](#api-versions)). Material sheets can be created since Ulysses 31 ([API version 3](#api-versions)).*</span>

**Parameters:**

-   `text`  
    The contents that should be inserted to the new sheet. Must be URL-encoded. Contents are imported as Markdown by default.
- `group`  
        Optional. Specifies the group the new sheet should be inserted to. This argument can be set to one of the following values:
    1. A group name (e.g. `My Group`) that will match the first group having the same name, regardless of its position in the group hierarchy.
    2. A path to a particular target group (e.g. `/My Group/My Subgroup`). Any path must begin with a slash. ([More…](#paths))
    3. A unique [identifier](#identifier) of the target group. ([More…](#identifier))
    4. If no value is given, the sheet is created inside the Inbox.
- `format`  
        Optional. Specifies the format of the imported text:  `markdown`, `text`, `html`. Defaults to Markdown.
- `index`  
        Optional. The position of the new sheet in its parent group. Use `0` to make it the first sheet.  
        <span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>
- `material`
        Optional. Whether or not the sheet should be created as material sheet. Defaults to `NO`.
        <span class="availability-version">*Available since Ulysses 31 ([API version 3](#api-versions)).*</span>

**Example:**

	ulysses://x-callback-url/new-sheet?text=My new sheet&index=2

<a id="new-group"></a>

### new-group
Creates a new group.

<span class="availability-version">*Available on iOS since Ulysses 2.6 ([API version 1](#api-versions)), on Mac since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

-   `name`  
    The name of the group to be created.
- `parent`  
        Optional. Specifies the parent group the new group should be inserted to. This argument can be set to one of the following values:
    1. A group name (e.g. `My Group`) that will match the first group having the same name, regardless of its position in the group hierarchy.
    2. A path to a particular target group (e.g. `/My Group/My Subgroup`). Any path must begin with a slash. ([More…](#paths))
    3. A unique [identifier](#identifier) of the target group. ([More…](#identifier))
    4. If no value is given, the group is created inside the top level group.
- `index`  
        Optional. The position of the new group in its parent group. Use `0` to make it the first group.  
        <span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Example:**

	ulysses://x-callback-url/new-group?name=My Group&index=3

<a id="insert"></a>

### insert

Inserts or appends text to a sheet.

<span class="availability-version">*Available on iOS since Ulysses 2.6 ([API version 1](#api-versions)), on Mac since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `id`  
  The [identifier](#identifier) of the sheet the text should be inserted to. ([More…](#identifier))
- `text`  
  The contents that should be inserted to the new sheet. Must be URL-encoded. Contents are imported as Markdown by default.
- `format`  
  Optional. Specifies the format of the imported text:  `markdown`, `text` or `html`. Defaults to Markdown.
- `position`  
  Optional. Set `begin` or `end` in order to insert text at the beginning of a document. If not given, the text is appended.
- `newline`  
  Optional. Specifies how newlines should be inserted to the text: Set `prepend` to prepend the inserted text by a newline. Set `append` to append the inserted text with a newline and `enclose` to enclose the entire text with newlines.

**Example:**

	ulysses://x-callback-url/insert?id=H8zLAmc1I0njH-0Ql-3YGQ&text=Inserted text

<a id="attach-note"></a>

### attach-note
Creates a new note attachment on a sheet.

<span class="availability-version">*Available on iOS since Ulysses 2.6 ([API version 1](#api-versions)), on Mac since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `id`  
  The [identifier](#identifier) of the sheet the note should be attached to. ([More…](#identifier))
- `text`  
  The contents that should be attached as note to the new sheet. Must be URL-encoded. Contents are imported as Markdown by default.
- `format`  
  Optional. Specifies the format of the imported text:  `markdown`, `text` or `html`. Defaults to Markdown.

**Example:**

	ulysses://x-callback-url/attach-note?id=H8zLAmc1I0njH-0Ql-3YGQ&text=My new note

<a id="update-note"></a>

### update-note
Changes an existing note attachment on a sheet. Requires [authorization](#authorization).

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `id`  
  The [identifier](#identifier) of the sheet where a note should be changed. ([More…](#identifier))
- `index`  
  The position of the note to change. Use `0` for the first note, `1` for the second note, and so on.
- `text`  
  The new contents of the note. Must be URL-encoded. Contents are imported as Markdown by default.
- `format`  
  Optional. Specifies the format of the imported text:  `markdown`, `text` or `html`. Defaults to Markdown.

**Example:**

	ulysses://x-callback-url/update-note?id=H8zLAmc1I0njH-0Ql-3YGQ&index=2&text=New note text&access-token=67be78e8b61e946dbc44fd5135ec99bf

<a id="remove-note"></a>

### remove-note
Removes a note attachment from a sheet. Requires [authorization](#authorization).

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `id`  
  The [identifier](#identifier) of the sheet from which a note should be removed. ([More…](#identifier))
- `index`  
  The position of the note to remove. Use `0` for the first note, `1` for the second note, and so on.

**Example:**

	ulysses://x-callback-url/remove-note?id=H8zLAmc1I0njH-0Ql-3YGQ&index=1&access-token=67be78e8b61e946dbc44fd5135ec99bf

<a id="attach-image"></a>

### attach-image
Creates a new image attachment on a sheet.

<span class="availability-version">*Available on iOS since Ulysses 2.6 ([API version 1](#api-versions)), on Mac since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `id`  
  The [identifier](#identifier) of the sheet the image should be attached to. ([More…](#identifier))
- `image`  
  The image data that should be used. The data must use [base64 encoding](https://wikipedia.org/wiki/Base64). Make sure that the base64-encoded data is also URL encoded.
- `format`  
  The format of the provided image (use an image path extension, like `png`, `pdf`, `jpg`, `raw`, `gif`). 
- `filename`  
  A filename the image format should be detected from. Either the argument `format` or `filename` must be provided.

**Example:**

	ulysses://x-callback-url/attach-image?id=H8zLAmc1I0njH-0Ql-3YGQ&image=iVBORw0KGgoAAAANSUhEUg…

<a id="attach-keywords"></a>

### attach-keywords
Adds one or more keywords to a sheet.

<span class="availability-version">*Available on iOS since Ulysses 2.6 ([API version 1](#api-versions)), on Mac since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `id`  
  The [identifier](#identifier) of the sheet the keywords should be attached to. ([More…](#identifier))
- `keywords`  
  A comma separated list of keywords that should be attached to the sheet.

**Example:**

	ulysses://x-callback-url/attach-keywords?id=H8zLAmc1I0njH-0Ql-3YGQ&keywords=Draft,Important

<a id="remove-keywords"></a>

### remove-keywords
Removes one or more keywords from a sheet. Requires [authorization](#authorization).

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `id`  
  The [identifier](#identifier) of the sheet the keywords should be removed from.  ([More…](#identifier))
- `keywords`  
  A comma separated list of keywords that should be removed from the sheet.

**Example:**

	ulysses://x-callback-url/remove-keywords?id=H8zLAmc1I0njH-0Ql-3YGQ&keywords=Draft,Important&access-token=67be78e8b61e946dbc44fd5135ec99bf

<a id="set-group-title"></a>

### set-group-title
Changes the title of a group. Requires [authorization](#authorization).

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

-   `group`  
        The group for which the title should be changed. This argument can be set to one of the following values:
    1. A group name (e.g. `My Group`) that will match the first group having the same name, regardless of its position in the group hierarchy.
    2. A path to a particular target group (e.g. `/My Group/My Subgroup`). Any path must begin with a slash. ([More…](#paths))
    3. A unique [identifier](#identifier) of the target group. ([More…](#identifier))
- `title`  
        The new title. Must be URL-encoded.

**Example:**

	ulysses://x-callback-url/set-group-title?group=My Group&title=The new title&access-token=67be78e8b61e946dbc44fd5135ec99bf

<a id="set-sheet-title"></a>

### set-sheet-title
Changes the first paragraph of a sheet. Requires [authorization](#authorization).

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

-   `sheet`  
        The [identifier](#identifier) of the sheet for which the title should be changed. ([More…](#identifier))
- `title`  
        The new title. Must be URL-encoded.
- `type`  
        The type of paragraph to use for the title. This argument can be set to one of the following values:
    1. `heading1`, …, `heading6` to use a heading paragraph.
    2. `comment` to use a comment paragraph.
    3. `filename` to use a filename paragraph such as `@: My Filename`. Only applies to sheets in external folders.

If the sheet has a first paragraph with the requested type, the paragraph contents will be changed (a heading replaces any existing heading). Otherwise, a new paragraph with the requested type and contents will be inserted at the beginning of the sheet.

**Example:**

	ulysses://x-callback-url/set-sheet-title?sheet=hZ7IX2jqKbVmPGlYUXkZjQ&title=The new title&type=heading4&access-token=67be78e8b61e946dbc44fd5135ec99bf

<a id="move"></a>

### move
Moves an item (sheet or group) to a target group and/or to a new position. Requires [authorization](#authorization).

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

-   `id`  
        The [identifier](#identifier) of the item (sheet or group) that should be moved. ([More…](#identifier))
- `targetGroup`  
        Optional if `index` is set. The group where the item should be moved. This argument can be set to one of the following values:
    1. A group name (e.g. `My Group`) that will match the first group having the same name, regardless of its position in the group hierarchy.
    2. A path to a particular group (e.g. `/My Group/My Subgroup`). Any path must begin with a slash. ([More…](#paths))
    3. A unique [identifier](#identifier) of a sheet or group that should be opened. ([More…](#identifier))  
       If the parameter is not set, the item will be moved inside of its current parent group.
- `index`
        Optional if `targetGroup` is set. The position of the item in its target group. Use `0` to make it the first item.

**Example:**

	ulysses://x-callback-url/move?id=hZ7IX2jqKbVmPGlYUXkZjQ&targetGroup=H8zLAmc1I0njH-0Ql-3YGQ&index=5&access-token=67be78e8b61e946dbc44fd5135ec99bf

<a id="copy"></a>

### copy
Copies an item (sheet or group) to a target group and/or to a new position.

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

-   `id`  
        The [identifier](#identifier) of the item (sheet or group) that should be copied. ([More…](#identifier))
- `targetGroup`  
        Optional if `index` is set. The group where the item should be copied. This argument can be set to one of the following values:
    1. A group name (e.g. `My Group`) that will match the first group having the same name, regardless of its position in the group hierarchy.
    2. A path to a particular group (e.g. `/My Group/My Subgroup`). Any path must begin with a slash. ([More…](#paths))
    3. A unique [identifier](#identifier) of a sheet or group that should be opened. ([More…](#identifier))  
       If the parameter is not set, the item will be copied inside of its current parent group.
- `index`  
        Optional if `targetGroup` is set. The position of the item in its target group. Use `0` to make it the first item.

**Example:**

	ulysses://x-callback-url/copy?id=hZ7IX2jqKbVmPGlYUXkZjQ&targetGroup=H8zLAmc1I0njH-0Ql-3YGQ&index=4

<a id="trash"></a>

### trash
Moves an item (sheet or group) to the trash. Requires [authorization](#authorization).

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `id`  
  The [identifier](#identifier) of the item (sheet or group) that should be moved to the trash. ([More…](#identifier))

**Example:**

	ulysses://x-callback-url/trash?id=hZ7IX2jqKbVmPGlYUXkZjQ&access-token=67be78e8b61e946dbc44fd5135ec99bf

<a id="get-item"></a>

### get-item
Retrieves information about an item (sheet or group). Requires [authorization](#authorization).

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `id`  
  The [identifier](#identifier) of the item (sheet or group) for which to retrieve information. ([More…](#identifier))
- `recursive`  
  Optional. Determines whether the result should include all sub-groups of the group. Only relevant if the item is a group. The allowed values are `YES` and `NO`. Defaults to `YES`.

**Results:**

The `x-success` callback retrieves an argument `item`. The value is a URL-encoded [JSON](http://www.json.org) object providing information on the requested item. The structure of this value depends on the item type. 

**Groups:**

If the item is a group, then the info object has this format:

- `title`  
  The title of the group.
- `type`  
  The type of the item. This can be either:
    - `group` – A regular group.
    - `filter`
    - `trash`
    - `project` - The root group of a project ([API version 3](#api-versions))
    - `projectMain` - The main content section of a project ([API version 3](#api-versions))
    - `projectExtras` - The extras section of a project ([API version 3](#api-versions))
    - `other` – Technical group type.
- `identifier`  
  The [identifier](#identifier) of the group.
- `hasLifetimeIdentifier`
  Whether the identifier stays the same even if the group is moved or renamed. `true` for items that are stored in the sections "iCloud" or "On My Mac" / "On My iPad". `false` for items in external folders or Dropbox.
- `containers`  
  The item descriptions for all child groups of this group. Will be empty if the parameter `recursive` was set to `NO`.
- `sheets`  
  The item descriptions for all sheets of this group. Will be empty for filters.

**Sheets:**

If the item is a sheet, then the object has this format:

- `title`  
  The sheet's title, limited to a length of 256 characters.
- `titleType`  
  The markup type of the title. Will be set to `heading1`…`heading6`, `comment` if the title is a heading or comment. Will be set to `filename` on external folders or Dropbox if the title is the sheet's filename (e.g. `@: My Filname`). If no title is given this vlaue is set to `null`.
- `type`  
  The type of the item (always `sheet`).
- `identifier`  
  The [identifier](#identifier) of the sheet.
- `hasLifetimeIdentifier`  
  Whether the identifier stays the same even if the group is moved or renamed. `true` for items that are stored in the sections "iCloud" or "On My Mac" / "On My iPad". `false` for items in external folders or Dropbox.
- `isMaterial`
  Whether the item is a material sheet (`true`) or a regular sheet (`false`). ([API version 3](#api-versions))
- `changeToken`  
  A value that identifies the current verison of the sheet. The change token will have a different value whenever the sheet changes.
- `modificationDate`  
  The timestamp when the sheet was last modified. The timestamp is given as the number of seconds relative to *00:00:00 UTC on 1 January 2001*.
- `creationDate`  
  The timestamp when the sheet was last created.

  
**Example:**

	ulysses://x-callback-url/get-item?id=H8zLAmc1I0njH-0Ql-3YGQ&recursive=NO&access-token=67be78e8b61e946dbc44fd5135ec99bf

<a id="get-root-items"></a>

### get-root-items
Retrieves information about the root sections. Can be used to get a full listing of the entire Ulysses library. Requires [authorization](#authorization).

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `recursive`  
  Optional. Determines whether the result should be a deep listing of the entire Ulysses library. The allowed values are `YES` and `NO`. Defaults to `YES`.

**Results:**

The `x-success` callback retrieves an argument `items`. The value is a URL-encoded [JSON](http://www.json.org) array. It contains an [info object](#get-item-results) for each section such as „iCloud”, „On My iPad”, and one for each external folder.

**Example:**

	ulysses://x-callback-url/get-root-items?recursive=NO&access-token=67be78e8b61e946dbc44fd5135ec99bf

<a id="read-sheet"></a>

### read-sheet
Retrieves the contents (text, notes, keywords) of a sheet. Requires [authorization](#authorization).

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `id`  
  The [identifier](#identifier) of the sheet that should be read. ([More…](#identifier))
- `text`  
  Optional. Determines whether the full text of the sheet should be included in the result. The allowed values are `YES` and `NO`. Defaults to `NO`.

**Results:**

The `x-success` callback retrieves an argument `sheet`. The value is a URL-encoded [JSON](http://www.json.org) object. It has the following structure:

- `title`  
  The sheet's title, limited to a length of 256 characters.
- `titleType`  
  The markup type of the title. Will be set to `heading1`…`heading6`, `comment` if the title is a heading or comment. Will be set to `filename` on external folders or Dropbox if the title is the sheet's filename (e.g. `@: My Filname`). If no title is given this vlaue is set to `null`.
- `type`  
  The type of the item (always `sheet`).
- `changeToken`  
  A value that identifies the current verison of the sheet. The change token will have a different value whenever the sheet changes.
- `modificationDate`  
  The timestamp when the sheet was last modified. The timestamp is given as the number of seconds relative to *00:00:00 UTC on 1 January 2001*`.
- `creationDate`  
  The timestamp when the sheet was last created.
- `identifier`  
  The [identifier](#identifier) of the sheet.
- `hasLifetimeIdentifier`  
  Whether the identifier stays the same even if the group is moved or renamed. `true` for items that are stored in the sections "iCloud" or "On My Mac" / "On My iPad". `false` for items in external folders or Dropbox.
- `isMaterial`
  Whether the item is a material sheet (`true`) or a regular sheet (`false`). ([API version 3](#api-versions))  
- `text`  
  The sheets content encoded as Markdown. This is only available if the `text` parameter was set to `YES`.
- `keywords`  
  The keywords of the sheet as an array of strings.
- `notes`  
  The notes of the sheet as an array of strings. All notes are encoded as Markdown.

**Example:**

	ulysses://x-callback-url/read-sheet?id=rQODhTD-dTrwM69Qrsj9cQ&text=YES&access-token=0215c79c391140aab65c3a15ceb61ee1

<a id="get-quick-look-url"></a>

### get-quick-look-url
Gets the QuickLook URL for a sheet. This is the sheet's location on the file system. Only available in Ulysses for Mac.

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

- `id`  
  The [identifier](#identifier) of the sheet for which a QuickLook URL should be generated.

**Results:**

The `x-success` callback retrieves an argument `url`. The value is the sheet's URL-encoded file path.

**Example:**

	ulysses://x-callback-url/get-quick-look-url?id=hZ7IX2jqKbVmPGlYUXkZjQ

<a id="open"></a>

### open
Opens an item (sheet or group) with a particular [identifier](#identifier) in Ulysses.

<span class="availability-version">*Available on iOS since Ulysses 2.6 ([API version 1](#api-versions)), on Mac since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Parameters:**

-   `id`  
        Specifies the item (sheet or group) that should be opened. This argument can be set to one of the following values:
    1. A group name (e.g. `My Group`) that will match the first group having the same name, regardless of its position in the group hierarchy.
    2. A path to a particular group (e.g. `/My Group/My Subgroup`). Any path must begin with a slash. ([More…](#paths))
    3. A unique [identifier](#identifier) of a sheet or group that should be opened. ([More…](#identifier))

**Notes:**

You can get a URL for opening sheets and groups directly within Ulysses. **On iOS**, touch and hold on any sheet, tap “Share” action, choose the “Copy Callback URL”. A URL for opening your sheet  has been now placed on your pasteboard. To get the callback URL for a group, open the group in the sheet list, tap the `…` button in the top right, select “Share” and “Copy Callback URL”. **On Mac**, select the sheet and press ⌘C (command-C). Alternatively, hold ⌥ (alt/option) while right-clicking a group in the Library, or a sheet in the sheet list. Select “Copy Callback URL” from the context menu.

**Example:**

	ulysses://x-callback-url/open?id=hZ7IX2jqKbVmPGlYUXkZjQ

<a id="open-group"></a>

### open-all, open-recent, open-favorites
Opens the special groups “All”, “Last 7 Days” and “Favorites”.

<span class="availability-version">*Available on iOS since Ulysses 2.6 ([API version 1](#api-versions)), on Mac since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

<a id="get-version"></a>

### get-version

Retrieves the build number of Ulysses, and the [version of Ulysses' X-Callback API](#api-versions).

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

**Results:**

The `x-success` callback retrieves two arguments:

- `apiVersion`: The version of Ulysses' X-Callback API (e.g. `3`). Use this to find out which actions (and parameters) are supported.
- `buildNumber`: The build version number of Ulysses itself (e.g. `32193`).

# Concepts

<a id="identifier"></a>

## Identifiers

Several callbacks use so-called identifier arguments to specify the sheet or group an operation should be executed on. Identifiers are 22 characters long and are in a URL-safe encoding. For example:

    ulysses://x-callback-url/open?id=DCj45UWKr_g15y2vBPwJdQ

will open the item with this identifier.

Identifiers are created internally by Ulysses and allow to reference sheets and groups. If you’re using iCloud or a local library, an identifier remains the same, even if the item is edited, renamed or moved around. In external folders, the identifier can change if you move or rename an item.

**On iOS**, you can get the x-callback identifier or URL of a sheet or a group by following these steps:

- Open the sheet list
- Touch and hold a sheet to open the menu
- Select the “Share” action
- Tap “Share Shortcuts Identifier” activity for the identifier, “Copy Callback URL” for the whole URL
- Tap “Copy”
- To get the identifier of a group, tap the `…` button in the top right of the sheet list, select “Share” and follow the same steps

**On Mac**, you can select the sheet in the sheet list and press ⌘C (command-C). Alternatively, hold ⌥ (alt/option) while right-clicking a group in the Library, or a sheet in the sheet list. Select “Copy Callback Identifier”/“Copy Callback URL” from the context menu.

The identifier is copied to the pasteboard. You can then paste it into a URL, like this:

	ulysses://x-callback-url/new-group?name=Best&parent=DCj45UWKr_g15y2vBPwJdQ

The identifier has a URL-safe encoding that can be directly passed to your x-callback action. The URL will work regardless of how you rename the item, or where you move it.

<a id="path"></a>

## Paths

As an alternative to identifiers, the target group of many actions can be also specified by a path of group names. Usually, a path refers to groups inside the sections “iCloud”, “On My iPad” or “On My iPhone”. However, if a path begins with the name of an external folder (e.g. Dropbox), Ulysses will resolve the path within this folder.

A path must begin with a slash character **/**. Ulysses will try to match the group names in the path with the original casing first. If the group is not found, Ulysses will fall back to case-insensitive matching.

**Example:**

- **/Books/Huckleberry Finn** matches a group named “Huckleberry Finn” inside the group “Books”.
- **/My Dropbox/Reports**: If a Dropbox folder “My Dropbox” exists, this path will match the folder “Reports” inside of it.

## x-callback Parameters
In addition to action parameters, there are some generic parameters you can provide **optionally** for any action. Many automation apps provide these arguments automatically:

-   `x-success`  
        URL that should be opened when an action has been successfully completed. If not provided, the user stays in Ulysses. The URL will retrieve at least the following arguments:
    1. `targetId`  
       The ID of the sheet or group that was either created, modified or revealed by the action.
    2. `targetURL`  
       A URL that can be used to open the sheet or group that has been created , modified or revealed by the action. Depending on the action, the URL may retrieve more arguments containing additional results.
- `x-error`  
        URL to be opened if an error occurred. It will retrieve the arguments `errorCode` and `errorMessage` providing further details on the error. Errors will mostly occur, if an identifier cannot be resolved. If not provided, the user stays in Ulysses if an error occurred.

**Example:**

If a new sheet should be created and Ulysses should return to the calling app, use the following URL (line breaks are for legibility):

	ulysses://x-callback-url/new-sheet?
	    x-success=sourceapp://x-callback-url/success&
	    group=Lecture Notes

On success, Ulysses will call the following URL to return to the calling app:

	sourceapp://x-callback-url/success?
	    targetId=H8zLAmc1I0njH-0Ql-3YGQ&
	    targetURL=ulysses://x-callback-url/open?id=H8zLAmc1I0njH-0Ql-3YGQ

<a id="authorization"></a>

## Authorization
To protect the Ulysses library against access from malicious apps, actions that expose content or destructively change require the calling app to be authorized.

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

Authorizing an app `My App` consists of these steps:

1. `My App` calls Ulysses' `authorize` action. The action has a required argument `appname`, which must be set to the name of the calling app. For example:  

	```
	ulysses://x-callback-url/authorize?appname=My App&x-success=myapp://x-callback-url/auth-success
	```
	
2. Ulysses asks the user to allow or deny `My App` access to the Ulysses library.
3. If the user allows access, Ulysses calls the `x-success` callback (in the example `myapp://x-callback-url/auth-success`) and sets the parameter `access-token`.
   If the user denies access, Ulysses calls the provided `x-error` callback.
4. `My App` stores the access token, and passes it to Ulysses when calling actions that require authorization. For example:  

	```
	ulysses://x-callback-url/get-item?id=H8zLAmc1I0njH-0Ql-3YGQ&recursive=NO&access-token=464b33b8ea994f2784dbedca60de0ebf
	```

<a id="silent-mode"></a>

## Silent Mode

By default, when an item is affected by an action (e.g. moved), Ulysses reveals it to the user. To prevent this, add the parameter `silent-mode=YES` to the URL.

<span class="availability-version">*Available since Ulysses 2.8 ([API version 2](#api-versions)).*</span>

On MacOS, an app can also prevent Ulysses switching to foreground. This can be done by setting the option `NSWorkspaceLaunchWithoutActivation` when opening the URL using `NSWorkspace`.

<a id="api-versions"></a>

## API Versions

Every Ulysses version from 2.8 onwards has an API version number, which can be retrieved using the [get-version](#get-version) action. The version indicates which actions and parameters can be called.

If you would like to distribute an app that communicates with the X-Callback API of Ulysses, please let it call [get-version](#get-version) first, and ensure that it only calls actions that are supported in the Ulysses version installed on the user's system.

The table below shows the API versions supported by different versions of Ulysses:

| API Version | Ulysses Version |
| ----------- | --------------- |
| 1           | 2.6, iOS only.  |
| 2           | 2.8             |
| 3           | 31              |

