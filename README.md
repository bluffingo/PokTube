# PokTube

![Website](screenshot.png)

This was the original FrameBit-based codebase used by squareBracket (now also known as FulpTube) back when it first launched in 2021. If you look deep enough within this repository's history, you will also find an earlier codebase from 2020.

This codebase was used for the first three months of squareBracket's existence. It was scrapped in favor of a new codebase (now OpenSB) on April 24th 2021.

This codebase is riddled with SQL injection vulnerabilities inherited from FrameBit, and additionally has questionable licensing, so we suggest not using this at all. This code remains up for archival purposes.

## How to set PokTube up (might not be proper)

1. Get XAMPP.
2. Install Apache and MySQL from the XAMPP Control Panel.
3. Get ``poktube.sql`` from here
4. Make a database called ``poktube``
5. Import ``poktube.sql`` to the ``poktube`` database
6. Make a folder called ``preload`` in the content folder if it does not exist.

### Database schema updates

#### April 23rd 2021 database changes
Adds support for toggling between current player and old March 2021 player.
```sql
ALTER TABLE `users` ADD `player` INT NOT NULL AFTER `is_partner`; 
```
#### April 22th 2021 database changes
This adds support for reporting when the video was last viewed, this is for the frontends (eg: VidLii, 2006, etc)
```sql
ALTER TABLE `videodb` ADD `LastViewed` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP AFTER `CustomThumbnail`; 
```
#### April 9th 2021 database changes
This adds support for the banning system.
```sql
ALTER TABLE `users` ADD `isBanned` BOOLEAN NOT NULL AFTER `password`; 
ALTER TABLE `users` ADD `banReason` TEXT NOT NULL AFTER `isBanned`; 
ALTER TABLE `users` ADD `bannedUntil` BIGINT UNSIGNED NOT NULL AFTER `banReason`; 
```
#### March 31st 2021 database changes
This should make reverse ordering work for members.php. it might not work, i swear. also extremely basic quickplay support
```sql
ALTER TABLE `users`
DROP PRIMARY KEY,
ADD PRIMARY KEY (`id`);
ALTER TABLE `users` CHANGE `id` `id` INT(64) NOT NULL AUTO_INCREMENT, add PRIMARY KEY (`id`); 

ALTER TABLE `users` ADD `quicklist` TEXT NOT NULL AFTER `subscriptions`; 
```
#### March 26th 2021 database changes
This adds support for subscriptions
```sql
ALTER TABLE `users` ADD `subscriptions` TEXT NOT NULL AFTER `subscribers`; 
```
#### March 19th 2021 database changes
This adds support for bulletins, as well as expanding customability for profiles.
```sql
CREATE TABLE `bulletins` ( `id` bigint(11) NOT NULL, `date` date NOT NULL, `subject` text NOT NULL, `body` text NOT NULL, `user` text NOT NULL );

ALTER TABLE `videodb` CHANGE `UploadDate` `UploadDate` DATETIME NOT NULL; 

ALTER TABLE `users` ADD `channel_inside` VARCHAR(255) NOT NULL DEFAULT '#EDF5FB' AFTER `channel_bg`;

ALTER TABLE `users` ADD `channel_text` VARCHAR(255) NOT NULL DEFAULT '#0033CC' AFTER `channel_inside`; 
```

#### March 6th 2021 database changes
This adds the length of videos.
```sql
ALTER TABLE `videodb` ADD `VideoLength` BIGINT UNSIGNED NOT NULL AFTER `HQVideoFile`;
```
#### February 22nd 2021 database changes
This adds the "Is admin" and the "Is approved" things, for the Admin Control Panel.
```sql
ALTER TABLE `users` ADD `is_admin` INT(4) NOT NULL DEFAULT '0' AFTER `is_partner`; 

ALTER TABLE `videodb` ADD `isApproved` INT(4) NOT NULL AFTER `UploadDate`; 
```
#### February 9th 2021 database changes
This adds Partner and HD Video Support. NEEDED OR ELSE UPLOADER WILL NOT WORK.
```sql
ALTER TABLE `users` ADD `is_partner` TINYINT NOT NULL AFTER `registeredon`; 

ALTER TABLE `videodb` ADD `HQVideoFile` TEXT NOT NULL AFTER `VideoFile`;
```
