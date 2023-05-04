Download Link: https://assignmentchef.com/product/solved-cs426-project-3-2
<br>






<h1>1.      Problem Definition</h1>




In this project, you will implement a parallel face recognition algorithm using idea of Local Binary Patterns. Local Binary Patterns is a simple but effective idea in texture analysis. It is also used for face recognition and gesture recognition applications.




<h1>2.      LBP idea &amp; implementation</h1>

You can find the data set in the following link. It is highly recommended to read the Readme.txt file first. <a href="https://drive.google.com/file/d/0B3nmpRSZ28SXbkhweFQ3ajNncXM/view?usp=sharing">https://drive.google.com/file/d/0B3nmpRSZ28SXbkhweFQ3ajNncXM/view?usp=sharing</a>

This link also have additional util.c and util.h files, they are explained further below. If you can’t reach this link, contact us.

The LBP approach uses two separate subsets of a given dataset. The first subset will be training set whereas the second one will be test set.

<ul>

 <li>There are 18 different persons’ pictures in our dataset with 20 pictures for each person that are taken under different light condition and with different gestures.</li>

 <li>First k pictures of each person will be training images and the remaining 20-k pictures of each person will be used as test images. o Pictures are named as the following format : person_id.photo_id.txt o Each file contains 2D matrices.</li>

</ul>

o Ex for k=10:

<ul>

 <li>training set for person 1: 1.1.txt, 1.2.txt, 1.3.txt, … , 1.10.txt</li>

 <li>test set for person 1: 1.11.txt, 1.12.txt, 1.13.txt, … , 1.20.txt</li>

</ul>

<ul>

 <li>All pictures in the dataset are gray scale images. Each pixel value will be an integer between 0 and 255.</li>

 <li>Each image is composed of 180×200 pixels.</li>

</ul>

Following algorithm is adapted from the algorithm given in Wikipedia:

<a href="http://en.0wikipedia.org/wiki/Local_binary_patterns">http://en.0wikipedia.org/wiki/Local_binary_patterns</a>

<h1>3.      Training Step</h1>

For each image in the dataset, you will apply the following steps:

<ul>

 <li>For each pixel in an i m a g e , compare the pixel with its 8 neighbors (on its left-top, leftmiddle, left-bottom, right-top, etc.). Follow the pixels along a circle, i.e. clockwise.</li>

 <li>Where the center pixel’s value is less than the neighbor’s value, write “1”. Otherwise, write “0”. This gives an 8-digit binary number.</li>

 <li>Convert this value into a decimal value.</li>

 <li>Compute the histogram, over the pixels, of the frequency of each “number” occurring (i.e., each combination of which pixels are smaller and which are greater than the center).</li>

</ul>

o You can read about histograms from the following link:

<ul>

 <li><a href="http://en.0wikipedia.org/wiki/Histogram">http://en.0wikipedia.org/wiki/Histogram</a></li>

</ul>




<ul>

 <li>As a result you will have 18×10=180 histograms, using the above example k=10.</li>

</ul>




<strong>                 </strong>

<ol start="4">

 <li><strong> Test Step </strong></li>

</ol>

In test you will follow the following steps:

<ul>

 <li>Create a histogram for the test image as described previously.</li>

 <li>Find the distance values between test image’s histogram and training histograms.</li>

 <li>Select the closest training histogram to determine the person.  In this homework you will use the following distance function:

  <ul>

   <li>a, b are two vectors of size d.</li>

  </ul></li>

</ul>




<ul>

 <li>distance(a,b) =</li>

</ul>




<ul>

 <li>If a<sub>i</sub> + b<sub>i </sub>= 0, you can assume the corresponding term to be 0 as well.</li>

</ul>

<strong><em> </em></strong>

<ol start="5">

 <li><strong> Implementation Details </strong></li>

</ol>

<ul>

 <li>First, you will implement the sequential version of LBP Face Recognition o 3 functions will be implemented

  <ul>

   <li><em>void create_histogram</em>(int * hist, int ** img, int num_rows, int num_cols)</li>

  </ul></li>

 <li>Creates a histogram for image given by int **img and returns histogram as int * hist

  <ul>

   <li><em>double distance</em>(int * a, int *b, int size)</li>

  </ul></li>

 <li>Finds the distance between two vectors</li>

 <li><em>int find_closest</em>(int ***training_set, int num_persons, int num_training, int size, int * test_image)</li>

 <li>Finds the closest histogram for test image’s histogram from training set histograms</li>

 <li>Returns person id of the closest histogram</li>

 <li>You will profile your code using gprof and try to find the functions that are needed to be parallelized.</li>

</ul>

o For gprof details, there are many online tutorials.

<ul>

 <li>Second, you will implement parallel version of LBP Face Recognition o Basically, you will insert OpenMP pragmas to parallelize your code.</li>

 <li>You will profile your code again using gprof.</li>

 <li>You can download and use the util.h and util.c files, they are in the link provided at the beginning of the file. In these files, 2D array allocate and free functions, file reading functions are implemented.</li>

 <li>Your program will take a single command line input k which will show the number of images that will be used as training images for each person.</li>

</ul>




<strong>                 </strong>

<ol start="6">

 <li><strong> Output Format </strong></li>

</ol>

You will print test results of your program in the following format.

<ul>

 <li>file_name test_result_person_id correct_person_id</li>

 <li>You will also print parallel execution time and sequential execution time as in previous homework.</li>

 <li>You will also print out the number of correct answers.  Ex:

  <ul>

   <li>txt 1 1</li>

  </ul></li>

</ul>

1.12.txt 1 1

<ul>

 <li>txt 1 1</li>

 <li>txt 2 2</li>

 <li>txt 3 2</li>

</ul>

Accuracy: T correct answers for Z tests

Parallel time: XX.XX ms

Sequential time: YY.YY ms

Notice that we are still using k=10 example. So for every person, first 10 images make up training data. Then we take a test image, let’s say 2.12.txt. We will create histogram for this image file. Than we will compare it with the training histograms of all people. Hopefully, result of the comparison will tell us that this image belongs to person 2. Closest histogram to 2.12.txt is expected to come from images between

2.1.txt and 2.10.txt. You are going to output both the correct result and your own prediction.


