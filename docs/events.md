## List of events

* File
  * PhapTQ\LaravelFilemanager\Events\FileIsUploading
  * PhapTQ\LaravelFilemanager\Events\FileWasUploaded
  * PhapTQ\LaravelFilemanager\Events\FileIsRenaming
  * PhapTQ\LaravelFilemanager\Events\FileWasRenamed
  * PhapTQ\LaravelFilemanager\Events\FileIsMoving
  * PhapTQ\LaravelFilemanager\Events\FileWasMoving
  * PhapTQ\LaravelFilemanager\Events\FileIsDeleting
  * PhapTQ\LaravelFilemanager\Events\FileWasDeleted
* Image
  * PhapTQ\LaravelFilemanager\Events\ImageIsUploading
  * PhapTQ\LaravelFilemanager\Events\ImageWasUploaded
  * PhapTQ\LaravelFilemanager\Events\ImageIsRenaming
  * PhapTQ\LaravelFilemanager\Events\ImageWasRenamed
  * PhapTQ\LaravelFilemanager\Events\ImageIsResizing
  * PhapTQ\LaravelFilemanager\Events\ImageWasResized
  * PhapTQ\LaravelFilemanager\Events\ImageIsCropping
  * PhapTQ\LaravelFilemanager\Events\ImageWasCropped
  * PhapTQ\LaravelFilemanager\Events\ImageIsDeleting
  * PhapTQ\LaravelFilemanager\Events\ImageWasDeleted
* Folder
  * PhapTQ\LaravelFilemanager\Events\FolderIsCreating
  * PhapTQ\LaravelFilemanager\Events\FolderWasCreated
  * PhapTQ\LaravelFilemanager\Events\FolderIsRenaming
  * PhapTQ\LaravelFilemanager\Events\FolderWasRenamed
  * PhapTQ\LaravelFilemanager\Events\FolderIsMoving
  * PhapTQ\LaravelFilemanager\Events\FolderWasMoving
  * PhapTQ\LaravelFilemanager\Events\FolderIsDeleting
  * PhapTQ\LaravelFilemanager\Events\FolderWasDeleted

## How to use
 * Sample code : [laravel-filemanager-demo-events](https://github.com/UniSharp/laravel-filemanager-demo-events)
 * To use events you can add a listener to listen to the events.

    Snippet for `EventServiceProvider`

    ```php
    protected $listen = [
        ImageWasUploaded::class => [
            UploadListener::class,
        ],
    ];
    ```

    The `UploadListener` will look like:

    ```php
    class UploadListener
    {
        public function handle($event)
        {
            $method = 'on'.class_basename($event);
            if (method_exists($this, $method)) {
                call_user_func([$this, $method], $event);
            }
        }

        public function onImageWasUploaded(ImageWasUploaded $event)
        {
            $path = $event->path();
            //your code, for example resizing and cropping
        }
    }
    ```

 * Or by using Event Subscribers

    Snippet for `EventServiceProvider`

    ```php
    protected $subscribe = [
        UploadListener::class
    ];
    ```

    The `UploadListener` will look like:

    ```php
    public function subscribe($events)
    {
        $events->listen('*', UploadListener::class);
    }

    public function handle($event)
    {
        $method = 'on'.class_basename($event);
        if (method_exists($this, $method)) {
            call_user_func([$this, $method], $event);
        }
    }

    public function onImageWasUploaded(ImageWasUploaded $event)
    {
        $path = $event->path();
        // your code, for example resizing and cropping
    }

    public function onImageWasRenamed(ImageWasRenamed $event)
    {
        // image was renamed
    }

    public function onImageWasDeleted(ImageWasDeleted $event)
    {
        // image was deleted
    }

    public function onFolderWasRenamed(FolderWasRenamed $event)
    {
        // folder was renamed
    }
    ```
