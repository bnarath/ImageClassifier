# ImageClassifier
Train and deploy an image classifier with the TensorFlow framework

# Download annotated images from [oxford vgg dataset](http://www.robots.ox.ac.uk/~vgg/data/pets/)
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

os.listdir('Data')
['images', 'annotations']
```
