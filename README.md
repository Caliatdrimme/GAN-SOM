# GAN-SOM

## cGAN
`./cGAN_code.ipynb` is a jupyter notebook to perform tests with cGAN:
- upload dataset or download dataset from the original cGAN repo
- train model or download pretrained model from the original cGAN repo
- perform tests
- show test results 

The notebook runs on Colab.

`./cGAN_results` is the folder with test results for:
- datasets: Day2night, Edges2shoes, Facades, Maps
- models: day2night, edges2shoes, facades_label2photo, map2sat, sat2map

Original cGAN (pix2pix) GitHub project: [original cGAN repo](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix)

## SOM

MiniSOM references:
https://github.com/JustGlowing/minisom

https://heartbeat.fritz.ai/introduction-to-self-organizing-maps-soms-98e88b568f5d

The script fits 3 SOM's:
- real (domain B)
- fake (domain B)
- combined (fake and real in domain B)

For the fake/real SOMs it compares the mappings by Jaccard similiarity (are the images that were grouped together when real still grouped together when faked?). It reports the mean and median of the similiarities by image pair.
For the combined it checks how many pairs of real-fake images are mapped the same (is SOM able to distinguish between fake and real images?). Reports the number of pairs that are grouped together (not necessarily across pairs) out of the total number of pairs. 

For each dataset, the results are 3 SOM maps and 1 text file with the metrics described above. The SOM maps are frequency maps of each 
node being the winner - you can think of it as showing frequency of clashes for a hash value - similar items have the same winner/hash value. 

Variables that can be played around:
- should the real domain A images be used in some kind of comparison?
- initialization of SOM (currently picks random samples from data)
- size of SOM (currently 5X5)
- number of iterations (currently equals the number of images in set)
- other metrics of comparison of results
- other SOM training stuff (like type of neighbourhood function and size of neighbourhood)


## Evaluation

Possible evaluations to show in the wiki page (add new ideas to this list):
1. Compare the number of clusters for the 3 different cases (real - fake - mix)
DONE. you can see this in the results files

2. Compare/show the location of clusters for the 3 different cases (real - fake - mix), to see how it changes
PENDING. A little complicated to do, maybe if we have time.

3. Number of couples that are clustered together both in the real and in the fake domain -> for this I suggest to use the metric I proposed (count the number of couple in each real cluster that are grouped together again in the fake domain), but there might be a smarter way
This is somewhat captured by the Jaccard similiarity metrics.
Jaccard similiarity for images measures how similar the groupings for each image are in the fake domain vs the real domain.
Jaccard similiarity of clusters measures how similar the clusters are between the fake and real domains. 
Not sure if these are enough to capture what you mean. Your added code should use the clusters_fake and clusters_real now. They are vectors of image indexes for each cluster, only containing unique elements. 

4. Number of images clustered together in the mix set:
    1. that belong to the same couple -> this will tell us if couples get clustered together
    DONE. See the results files.
    2. that belong to the same set (real - fake) -> this will tell us whether SOM is able to group real and fake images separately
    Not sure if this is enough but so far I have added the number of clusters that have solely real images and number of clusters that have solely fake images. 

Since we have multiple datasets to test, in the wiki page we can visually compare these evaluations across the datasets. For instance we can make an histogram showing the results of point 1. in the different datasets.

