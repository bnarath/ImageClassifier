# ImageClassifier
Train and deploy an image classifier with the TensorFlow framework

## Download annotated images from [oxford vgg dataset](http://www.robots.ox.ac.uk/~vgg/data/pets/)
- They have 37 category pet dataset with roughly 200 images for each class. 
- The images have a large variations in scale, pose and lighting.
- All images have an associated ground truth annotation of breed, head ROI, and pixel level trimap segmentation.

```python
    def download_and_extract(data_dir, download_dir):
        #Download the data from urls and extract it to datadir
        for url in urls:
            filename = url.split('/')[-1]
            if filename not in os.listdir(download_dir):
                print(f'Downloading {filename}')
                print(os.path.join(download_dir, filename))
                try:
                    urllib.request.urlretrieve(url, filename=os.path.join(download_dir, filename))
                    tf = tarfile.open(name=filename)
                    print(tf)
                    tf.extractall(data_dir)
                except Exception as e:
                    print(e.args)
            else:
                print(f'{filename} is already downloaded')

    download_and_extract('Data', '.')

```


```diff
    Downloading images.tar.gz
    ./images.tar.gz
    <tarfile.TarFile object at 0x7f1a33257f60>
    Downloading annotations.tar.gz
    ./annotations.tar.gz
    <tarfile.TarFile object at 0x7f1a32665c50>
```



## Annotation
- Convert "Abyssinian_100 1 1 1" to "Abyssinian.jpg" and "cat"
- Perform the same for all lines in trainval and test

```python
    def annotate(file, annotations={}):

        with open(file, 'r') as f:
            rows = f.read().splitlines()

        for row in rows:
            image, class_id, species, breed = row.split()
            class_name = '_'.join(image.split('_')[:-1])
            image = image + '.jpg'

            annotations[image] = 'cat' if class_name[0] != class_name[0].lower() else 'dog'

        return annotations

    annotations = annotate('../Data/annotations/trainval.txt')
    annotations = annotate('../Data/annotations/test.txt')
```

## Train Validation Split
- Create new folder stucture under train_test_data and move each image to corresponding folders
- Train Validation split - 80%-20%

```python
    new_directory = '../train_test_data'
    classes = ['cat', 'dog']
    sets = ['train', 'validation']

    #Create folder structure if not present
    if not os.path.isdir(new_directory):
        os.mkdir(new_directory)

    for set_name in sets:
        to_create = os.path.join(new_directory, set_name)
        if not os.path.isdir(to_create):
            os.mkdir(os.path.join(new_directory, set_name))


        for class_name in classes:
            to_create = os.path.join(new_directory, set_name, class_name)
            if not os.path.isdir(to_create):
                os.mkdir(os.path.join(new_directory, set_name, class_name))
   
  #Move images to respective folders
  for image, class_name  in annotations.items():
    target_set = 'validation' if random.random() <= 0.2 else 'train'
    dest_path = os.path.join(new_directory, target_set, class_name, image)
    _=shutil.copy(os.path.join('../Data/images', image), dest_path)

```

