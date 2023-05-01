Download Link: https://assignmentchef.com/product/solved-csc3022h-assignment-4
<br>
<h1></h1>

<h1>                            Input                                                        Mask                                                      Output</h1>

Write an <strong><em>Image class</em></strong> which supports simple operations on pairs of <em>PGM</em> images. An image is a 2D array of numbers, each of which corresponds to the intensity of a pixel on the screen. So, essentially you will be writing a program that will manipulate 2D arrays of numbers. For any pair of “source” images, your program must support the following image operations, which generate a new output image:

<ol>

 <li>Add I1 I2 : add the pixel values of I1 to I2 (i.e. at every corresponding 2D position you add the two values)</li>

 <li>Subtract I1 I2 : subtract pixel values of I2 from I1</li>

 <li>Invert I1 : replace each pixel value p with (255 – p) <strong>NOTE: ONE image</strong> <strong>only</strong></li>

 <li>Mask I1 I2 : given I1 and an image I2, copy across values from I1 where I2 has a value of 255. All other output values are set to 0. An example of ‘masking’ is shown above.</li>

 <li>Threshold I1 f : for all pixels in I1 &gt; f, set the result to the integer 255, otherwise set the value to 0. You can build a mask using this function.</li>

</ol>

The program will be driven from the command line, so you will need to use <strong>argv</strong> and <strong>argc</strong> in the main() function to process the parameters the user enters when running the program. The options are of the form (I1 and I2 represent image file names):

<ul>

 <li>-a I1 I2 (add I1 and I2)</li>

 <li>-s I1 I2 (subtract I2 from I1)</li>

 <li>-i I1 (invert I1)</li>

 <li>-l I1 I2 (mask I1 with I2)</li>

 <li>-t I1 f (threshold I1 with integer value f)</li>

</ul>

The command line form will thus be

<strong>imageops</strong> &lt;option&gt; <em>OutputImageName</em>

where <strong>imageops</strong> is the name of your program, &lt;option&gt; represents the <strong><em>single</em></strong> operation to be performed (see above) and <em>OutputImageName</em> is the name for the result image. Note that only one option should be specified when you run the program.

When implementing this assignment, you should create a class called <strong>Image</strong> to wrap your image operations and data. The image data will be a block of width x height <strong>unsigned char</strong> values.

<strong>Requirements:</strong>

The requirements are as follows:

<ul>

 <li>Implement an appropriate constructor and destructor for your class. Use a std::unique_ptr&lt;&gt; to manage the space you have allocated. Remember you can use the <strong>get()</strong> method to obtain a raw pointer for data access.</li>

 <li>Implement appropriate move/copy semantics for your <strong>Image</strong> class along with appropriate constructors and destructor.</li>

 <li>Implement functions to support the image operations noted above, using operator overloading. Specifically

  <ul>

   <li>+ : addition of two images (I1 + I2)</li>

   <li>– : subtraction of two images (I1 − I2)</li>

   <li>! :Invert an image (!I2)</li>

   <li>/ :mask I1 with I2 (I1 / I2)</li>

   <li>*:threshold with f (int) (I1 * f)</li>

   <li>Implement I/O operators for <strong>Image</strong>: &lt;&lt; and &gt;&gt;</li>

  </ul></li>

</ul>

<strong>Make sure you only allow “binary” operations on images with the same size</strong>.

<ul>

 <li>Implement load()/save() methods for your Image class. More information on this is given in the notes below. You need only support simple PGM (1 byte) images which are a very simple image format.  Both these methods will take a string argument which is the file to be opened or written to.</li>

 <li>Image container: you must create an iterator for your <strong>Image</strong> This iterator should be used to gain access to the underlying image data and will support the * and ++/– operators. Appropriate begin() and end() methods must be written for <strong>Image</strong> to return a reference to an iterator to which these operators can be applied. We have provided a code skeleton showing some features of this “nested class” – you must fill in the remaining functionality. Note that it requires the use of <strong>friend</strong> to work correctly.  <strong>Your algorithms for image operations should be expressed in terms of these iterators. </strong></li>

</ul>

If you have implemented this correctly, you should be able to write code like the following:

void Image::copy(const Image&amp; rhs)

{

Image::iterator    beg = this-&gt;begin(), end = this-&gt;end();    Image::iterator    inStart = rhs.begin(), inEnd  = rhs.end();

while ( beg  !=  end) { *beg = *inStart;   ++beg; ++inStart; }

}

Notes:

<ul>

 <li>Image data is of type <strong>unsigned char</strong> (0…255)</li>

 <li>All result data must be clamped to this range i.e. &lt; 0 is set to 0 and &gt; 255 is set to 255.</li>

 <li>You must write a loop to extract the command line parameters first, this will be in main().</li>

 <li>You <strong><em>may not</em></strong> use a library to handle I/O for PGM files. You must write this code yourself.</li>

</ul>

This will be used to build the appropriate Image objects. Then you will need to perform the requested operation on the two input images and generate the named output image.

<h1>Image Class iterator</h1>

The iterator should be a nested class defined within Image. This means it lives within the “namespace” Image, and is thus logically associated. It is however, still separate class and cannot access the private members/data of Image. To get around this Image needs to be declared a “friend” of iterator. The following code snippet provides some of the structure for this class – you must fill in the rest and provide appropriate begin()/end() methods for Image to get the start and end iterators to the contained data.  You do not need to provide support for  “const_iterator”.

class Image {   private:     int width, height;

std::unique_ptr&lt;unsigned char[]&gt; data;   public:

class iterator    {      private:

unsigned char *ptr; // construct only via Image class (begin/end) iterator(u_char *p) : ptr(p) {}      public:

//copy construct is public iterator( const iterator &amp; rhs) : ptr(rhs.ptr) {}

// define overloaded ops: *, ++, –, = iterator &amp; operator=(const iterator &amp; rhs) {…}

// other methods for iterator

};

// define begin()/end() to get iterator to start and

// “one-past” end.

iterator begin(void) { return iterator(data.get())} // etc };

<h1>PGM Images</h1>

PGM images are <em>greyscale</em> – meaning they have no colour, and use only one value to encode an intensity that ranges from (black = 0) through to

(white=255). Each value is thus stored as an `unsigned char’. The images I will provide are  `raw’ PGM – i.e. binary files. However, they have a text header, so you usually open the file as binary (ios::binary), but use  the <strong>&gt;&gt;</strong> and <strong>&lt;&lt;</strong> operators to read/write the header information. Getline() is very useful for reading and discarding comment lines.  The header is followed by a block of bytes, which represent the image intensities. You can use the <strong>read()</strong> and <strong>write() </strong>methods to manipulate this data. Look at the function prototypes to see what arguments they expect (<strong>www.cplusplus.com</strong> can help here if you have no C++ reference). The PGM images you will receive will have the format:

<strong>P5</strong>

<strong># comment line (there can be more than 1 of these comment lines) # the string P5 will always be the first item in the file.</strong>

<strong>Nrows  Ncols 255 binary_data_block</strong>

where <em>Nrows</em> and <em>Ncols</em> are integers representing the rows and columns making up the image matrix.  There is a newline after each line – uses “ws” to process this correctly. After reading 255 (and using the <strong>ws </strong>manipulator)  you will be at the start of the binary data (Nrows*Ncols  <strong>unsigned chars</strong>) which you can read in using <strong>read().</strong>

Remember that image data (being a 2D array of <strong>unsigned char</strong> ) is indexed from [0][0] and that this is the <em>top left hand corner</em> of the image. Generally we read in the image data line by line, starting from the top. <strong><em>This is in fact how the binary image data is stored in the PGM format</em></strong> – so you can read/write the entire image with one read()/write() statement! This is very convenient.

We will provide some PGM images. You can view the effects of your image operations using an image viewer like <strong>Gimp</strong> on Ubuntu.

If you satisfactorily complete everything requested above you can score a maximum of 90%. An additional 10% of the prac mark can be obtained by adding the following functionality.

<strong>Extra credit:</strong>

Implement a further “filter” operator (%) for your image which will apply a filter g to the image:

Img % g

This must create a new image which is a filtered version of the input, Img. A filter (in this context) is a N X N array of floating point values, N odd, which are used to transform the input image. You ca create struct/class to represent this data. For example,  a 3×3 blurring filter might be

1/16 1/8 1/16

1/8 1/4 1/8

1/16 1/8 1/16

The filter will be provided in a text file of format   N  val1 val2 val3 ….

where N (an odd integer) is the filter size and the remaining N*N values are real numbers for the entries, in row major order.

When filtering, you turn each input pixel into a new filtered pixel, as follows:

<ul>

 <li>Place the centre of the array on the pixel you wish to filter</li>

 <li>Compute a <strong>weighted sum</strong> which includes all pixels covered by the grid of values (including the pixel in the centre). The weights are the real number at the corresponding positions. Multiply each weight with the corresponding pixel value and add all of this up. This can be done through a simple nested loop – you just need to figure out the indexing.</li>

 <li>write out the resulting value into the same pixel position in your <strong>output </strong></li>

 <li>When filtering values close to the boundary of the image, the grid will “fall off”. In these cases, just reflect the values from the inside around the relevant edge e.g. image[row][col] = image[row][2*width-col-1] if col goes off the righthand edge.</li>

</ul>

<strong>NOTE: you write the filtered output pixels to a new image! You treat the input image as a source for reading only.</strong>

<strong>Testing and Source Control</strong>

You should use <strong>catch.hpp</strong> to unit test your methods. To achieve this it may be easier to write a constructor that takes in width, height and a buffer of values, instead of reading these from file. Special care should be taken to test the following:

<ul>

 <li>Move and copy semantics</li>

 <li>Iterator and its operators (including boundary conditions)</li>

 <li>Thresholding, inverting and masking operator overloads</li>

 <li>Addition and subtraction of images. Ensure that the pixels are clamped appropriately.</li>

 <li>[optionally] the filtering operator. Take special care to test the boundary conditions</li>

</ul>

It may be easiest to compile your unit tests into separate executable(s) and run these tests when the user types “make test”. Be explicit in your readme about how these test cases should be executed.

A simple qualitative test for your program may involve the following sequence of operations:

<ol>

 <li>create an image mask M1 (via thresholding)</li>

 <li>create an inverted mask of M1 called M2 (by inverting M1)</li>

 <li>use M1 and an Image U1 to create a masked image</li>

 <li>use M2 and an Image U2 to create a second masked image</li>

 <li>add the two masked images together to create a new image with piecesof both U1 and U2!</li>

</ol>

You can use whatever images you want for U1, U2 and M1, but they MUST have the same dimensions.

Please ensure that you make use of version control for your assignment and that you commit small units of work regularly. Your commit messages should be detailed.


