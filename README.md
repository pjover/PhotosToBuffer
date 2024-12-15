# PhotosToSocial

PhotosToSocial is a python script to publish post with photos into social media (WordPress and BluSky by now). The goal
is to be able to post at least a photo every day without effort.

The workflow is like this:

1. Export the photos from lightroom or your favourite program into `PHOTOS_TO_SOCIAL_HOME` directory.
2. Load (running `main.py --load`) loads photos from home directory and prepare posts.
    1. `PhotoLoader` will load new photos and read metadata. This process will load new photos, comparing with posts
       already loaded ans stored into `POSTS_FILENAME` json file.
    2. `PostBuilder` creates post from photos
        - Metadata:
            - `Title` will be added to the post text. If multiple photos in the same job share the same `title`, then it
              will merged in a single post with all photos.
            - `Caption` will be added to the post text. If multiple photos are grouped in the same post, the `caption`
              field will be used for the post text, appending the caption of every photo to the post's text, and
              as `alt` text of the image (if it has content).
            - `Keywords` starting with `#` will be added to the post. If multiple photos are grouped in the same post,
              duplicated keywords are ignored.
        - The posts are stored into `POSTS_FILENAME` json file.
3. Post (running `main.py --post`) sends next post to Social media. This option is intendet to be called by a daily cron
   job.
    1. Reads next post to be published from `POSTS_FILENAME` json file.
    2. Send the post to social media, currently to BlueSky and WordPress (using Post by Email feature).
    3. Updates the post at `POSTS_FILENAME` json file.

## Configuration

Set these environment variables to set up the script:

- `PHOTOS_TO_SOCIAL_HOME`: Home directory were photos and `POSTS_FILENAME` json file are stored.
- `BLUE_SKY_USERNAME`: Your BlueSky username.
- `BLUE_SKY_PASSWORD`: Your BlueSky password.
- `GMAIL_USER_EMAIL`: Your gmail email.
- `GMAIL_APP_PASSWORD`: An gmail app password.
- `WORD_PRESS_POST_BY_EMAIL_TO`: the recipient address configured for WordPress Post by Email.
