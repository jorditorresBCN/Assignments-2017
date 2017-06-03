# Lab 6: Advanced Analytics as a Service in the Cloud

During this hands on we are going to experiment the power of the Cloud Vision API from Google to detect text within images as an example of Advanced Analytics as a Service in the Cloud. 
This hands-on follows the official introduction to text detection that is available [here]( https://github.com/GoogleCloudPlatform/cloud-vision/tree/master/python/text).
This hands-on detect text within images, stores this text in an index, and then lets you query this index. [Cloud Vision API](https://cloud.google.com/vision/) enables developers to understand the content of an image by encapsulating powerful machine learning models in an easy to use REST API. It quickly classifies images into thousands of categories (e.g., "sailboat", "lion", "Eiffel Tower"), detects individual objects and faces within images and finds and reads printed words contained within images. You can build metadata on your image catalog, moderate offensive content, or enable new marketing scenarios through image sentiment analysis. Analyze images uploaded in the request or integrate with your image storage on Google Cloud Storage. Try the API [here (Drag image file here or Browse from your computer)](https://cloud.google.com/vision/).

This example uses `TEXT_DETECTION` Vision API requests to build an inverted index from the stemmed words found in the images, and stores that index in a Redis database. The example uses the nltk library already discused in our labs for finding stopwords and doing stemming. The resulting index can be queried to find images that match a given set of words, and to list text that was found in each matching image.
## 6.1 Cloud Platform sign up
The first step is to [sign up]( https://cloud.google.com/vision/). It exists a free trial for this service, to fully enjoy the benefits of Google’s Cloud platform the best option is to get a business trial, with $300 worth of free credit for this year. That amount is far more than enough for the expected needs on this subject’s scope and will give you time enough to explore other features. Once the registration process is finished, we need to access the [management console]( https://console.cloud.google.com/home), where it is possible to create a new project, choosing a name and an ID for it. It is necessary to enable billing for this project to continue, but as long as we are spending from the free credit we have there is nothing to worry about.
The project creation process takes a few seconds, after finishing it will appear in our list. It is time to select the new project and enable the Cloud Vision API that is the tool we will use during this hands-on. Finally, it is needed to set up some credentials that will be the way we authenticate to enable the communication: This can be done selecting “Credentials” in the left menu and simply clicking over the blue button saying “Create credentials” and choosing the “service account key” option.  Select JSON as your key type. Once completed, your service account key is downloaded to your browser's default location.

## 6.2 Environment setup
The base language is Python and those packages and almost every needed module will be already installed. The scripts and demo files needed can be downloaded from the official Google Cloud Platform GitHub repository, and only a few more changes will be needed to be ready.

### Download the Example Code

Download the code from this repository.
If you have [`git`](https://git-scm.com/) installed, you can do this by executing the following command:

```$ git clone https://github.com/GoogleCloudPlatform/cloud-vision.git```

This will download the repository of samples into the directory
`cloud-vision`.

Otherwise, github offers an
[auto-generated zip file](https://github.com/GoogleCloudPlatform/cloud-vision/archive/master.zip) of the
`master` branch, which you can download and extract.

Either method will include the desired subdirectory
`python/text`.  Change to the `python/text` directory under `cloud-vision`.
The rest of the tutorial assumes this as your working directory.


This example has been tested with Python 2.7 and 3.4.

### Set Up to Authenticate With Your Project's Credentials

Next, set up to authenticate with the Cloud Vision API using your project's
service account credentials. E.g., to authenticate locally, set
the `GOOGLE_APPLICATION_CREDENTIALS` environment variable to point to your
downloaded service account credentials before running this example:

```export GOOGLE_APPLICATION_CREDENTIALS=/path/to/your/credentials-key.json```

If you do not do this, you will see an error that looks something like this when
you run the script:
`HttpError 403 when requesting
https://vision.googleapis.com/v1/images:annotate?alt=json returned
"Project has not activated the vision.googleapis.com API. Please enable the API
for project ..."`

### Install and Start Up a Redis server

This example uses a [redis](http://redis.io/) server, which must be up and
running before you start the indexing.  To install Redis, follow the
instructions on the [download page](http://redis.io/download), or install via a
package manager like [homebrew](http://brew.sh/) or `apt-get` as appropriate
for your OS.

The example assumes that the server is running on `localhost`, on the default
port, and it uses [redis
dbs](http://www.rediscookbook.org/multiple_databases.html) 0 and 1 for its data.
Edit the example code before you start if your redis settings are different.

## Quick Start: Running the Example ##

If you'd like to get the example up and running before we dive into the
details, here are the steps.

1. Ensure that your redis server is running.
2. Index a directory of images by passing the image name as an argument to
   `textindex.py`:

```
$ ./textindex.py <path-to-image-directory>
```

3. Wait for the indexing to finish.  The process is resumable, in case the
   indexing is interrupted.
4. Query the index to find images that match a given set of keywords.  An easy
   way to do this is from the Python interpreter, as shown below. Pick a word
   that you know is in your images.  If you list multiple words, the conjunction
   of hits will be returned— that is, images that contain all the given
   keywords.

```shell
$ python
...
>>> import textindex
>>> index = textindex.Index()
>>> index.print_lookup('cats')
...
>>> index.print_lookup('cats', 'dogs')
```
