# PhotosToBuffer

PhotosToBuffer is a tool to publish post with photos into [Buffer](https://buffer.com). The goal is to be able to post at least a photo every day without effort.

The workflow is like this:

1. Export the photos from lightroom or your favourite program. Photo's metadata will be used for:
   - `Title` will be added to the post text.
   - `Additional Info` will be added to the post text.
   - `Event` will be used to group photos in the same post, all photos will be grouped by `Event` content (if it is not empty); you can put there any text you want, is only used for grouping.
   - `Keywords` starting with `#` will be added to the post.
2. Importer
   - Import new photos reading a configured directory.
   - Identify the new photos.
   - Store the state in the db.
3. Processor
   - Read the state from the db.
   - Extract metadata:
     - `Caption`
     - `Additional Info`
     - `Event`
     - `Keywords` starting with `#`
   - Remove all metadata from the photos except:
       - `Caption`
       - `Additional Info`
       - `Event`
       - `Keywords` starting with `#`
       - `Creator`
       - `Copyright`
       - `Copyright Status`
       - `Rights Usage Terms`
       - `Copyright Info URL`
       - `E.Mail`
   - Update photos.
   - Update the state in the db.
4. Scheduler
   - Read the state from the db.
   - Identify pending groups to be published.
   - Set publishing date.
   - Update the state in the db.
5. Publisher
   - Read the state from the db.
   - Schedule next batch into every configured Buffer channel, keeping the max scheduled posts under the free tier limit.

We will store an read from database to be able to keep track of the state of every photo, and run the entire process even if there are no new photos, but som of them can be scheduled and published.

The database is implemented with SQLite.

