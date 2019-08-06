# Assignment 1B

### Q. What are Channels and Kernels?
**Ans.** Images are made up of the different features. These features when we extract individually, are know as channels and the extractor used for the process is called Kernel. 

#### Channels: 
Channels are the features of an image. Each channel contribute in making of final image. Channels contain similar type of information which is used collectively in identification of image.

`Intuition` - If you want to identify a person by his/her face, you eyes have to pass the information  regarding the human face to the brain. Information sent be like person's hair, hair color, hair style, shape of the nose, color of eyes, skin color, eyebrows, small/big forehead etc.. These individual information collectively helps a person to identify another person. These informations are called channels.

![4 channel image](https://i.imgur.com/TVhRSOH.png)

#### Kernels:  
A machine cannot understand an image like a human does. For a machine, image is like a 2-dimensional arrays of numbers, known as pixels.  

Machines do not require complete information of the image to identify it. Iamge can be identified with relevant features itself. So researchers proposed a approach to extract necessary information from the image. Dot product of a matrix with certain weights (numbers) is to be performed to find out the important features in the image. If a part of image is crucial in the identification, it would be provided with higher weights than the other pixel values. This process is know as convolution.  

These extracted features are stored in sequences of patterns, in sequential order. This hierarchy helps machine to identify the image `(edges -> texture -> pattern -> part_of_image-> image)`. Features are not extracted as one single number, there are extracted in small-small chunks. The extraction of feature is performed by a `specific sized weight matrix`, that matrix is known as **Kernel**.

`Example` - 3x3 kernel, 5x5 kernel, 7x7 kernel etc.

![How machine sees the image](https://i.imgur.com/oRKtQBv.png)  

![3x3 Kernel](https://i.imgur.com/F6Y88rQ.png)


### Q. Why should we only (well mostly) use 3x3 Kernels?
**Ans.** Kernels are used to extract features from the image. Each kernel has weights associated with them. These weights are learnt during the training (backpropogation) of the network. Learning of accurate weights is critical for model to perform and as the channels and number of layers increase, number of weights to be learnt, increase drastically. So it is better to maintain the kernel weight as minimum as possible.  

3x3 kernel perfom well as a feature extractor with only 9 weights to be learnt during network training. Also more than one 3x3 kernel can be used to get the same receptive field obtained from the bigger kernels like 5x5 or 7x7, with lesser number of trainable parameters. Below is comparative study of different kernel parameters requirement - 

| Kernel Size | Trainable Parameters | 3x3 kernel required to achieve same receptive field | Parameters with 3x3 implementation | Parameter additional to bigger kernel | #% lesser parameter required |
| :---------: | :------------------: | :-------------------------------------------------: | :--------------------------------: | :-----------------------------------: | :--------------------------: |
|     3x3     |          9           |                          1                          |                 9                  |                   0                   |             0.0              |
|     5x5     |          25          |                          2                          |                 18                 |                   7                   |             28.0             |
|     7x7     |          49          |                          3                          |                 27                 |                  22                   |             44.9             |
|     9x9     |          81          |                          4                          |                 36                 |                  45                   |            55.56             |
|    11x11    |         121          |                          5                          |                 45                 |                  76                   |            62.81             |

This study justifies the use of 3x 3 kernels.


### Q. How many times do we need to perform 3x3 convolution operation to reach 1x1 from 199x199 (show calculations)
**Ans.** When 3x3 convolution is performed over an image without padding (p) or stride (s) then its dimension goes down by 2 points and output images's global receptive field increase by the same amount.

**Formula to get the ouput dimension after convolution operation**  
`n_out = (n_in + 2p - k)/s + 1`

where
 > n_out = output dimension of the image  
 > n_in = input dimension of the image  
 > p = padding (default = 0)  
 > s = stride (default = 1)  

As per the calulation, for an image size of 199, it would take `100 layers` to reduce it to 1x1 image. Calculation is shown in the below table- 


| Layer Number | Input Shape | Layer Type | Kernel Size | Output Shape | Receptive Field |
| :----------: | :---------: | :--------: | :---------: | :----------: | :-------------: |
|      1       |   199x199   |  Convolve  |     3x3     |   197x197    |       3x3       |
|      2       |   197x197   |  Convolve  |     3x3     |   195x195    |       5x5       |
|      3       |   195x195   |  Convolve  |     3x3     |   193x193    |       7x7       |
|      4       |   193x193   |  Convolve  |     3x3     |   191x191    |       9x9       |
|      5       |   191x191   |  Convolve  |     3x3     |   189x189    |      11x11      |
|      6       |   189x189   |  Convolve  |     3x3     |   187x187    |      13x13      |
|      7       |   187x187   |  Convolve  |     3x3     |   185x185    |      15x15      |
|      8       |   185x185   |  Convolve  |     3x3     |   183x183    |      17x17      |
|      9       |   183x183   |  Convolve  |     3x3     |   181x181    |      19x19      |
|      10      |   181x181   |  Convolve  |     3x3     |   179x179    |      21x21      |
|      11      |   179x179   |  Convolve  |     3x3     |   177x177    |      23x23      |
|      12      |   177x177   |  Convolve  |     3x3     |   175x175    |      25x25      |
|      13      |   175x175   |  Convolve  |     3x3     |   173x173    |      27x27      |
|      14      |   173x173   |  Convolve  |     3x3     |   171x171    |      29x29      |
|      15      |   171x171   |  Convolve  |     3x3     |   169x169    |      31x31      |
|      16      |   169x169   |  Convolve  |     3x3     |   167x167    |      33x33      |
|      17      |   167x167   |  Convolve  |     3x3     |   165x165    |      35x35      |
|      18      |   165x165   |  Convolve  |     3x3     |   163x163    |      37x37      |
|      19      |   163x163   |  Convolve  |     3x3     |   161x161    |      39x39      |
|      20      |   161x161   |  Convolve  |     3x3     |   159x159    |      41x41      |
|      21      |   159x159   |  Convolve  |     3x3     |   157x157    |      43x43      |
|      22      |   157x157   |  Convolve  |     3x3     |   155x155    |      45x45      |
|      23      |   155x155   |  Convolve  |     3x3     |   153x153    |      47x47      |
|      24      |   153x153   |  Convolve  |     3x3     |   151x151    |      49x49      |
|      25      |   151x151   |  Convolve  |     3x3     |   149x149    |      51x51      |
|      26      |   149x149   |  Convolve  |     3x3     |   147x147    |      53x53      |
|      27      |   147x147   |  Convolve  |     3x3     |   145x145    |      55x55      |
|      28      |   145x145   |  Convolve  |     3x3     |   143x143    |      57x57      |
|      29      |   143x143   |  Convolve  |     3x3     |   141x141    |      59x59      |
|      30      |   141x141   |  Convolve  |     3x3     |   139x139    |      61x61      |
|      31      |   139x139   |  Convolve  |     3x3     |   137x137    |      63x63      |
|      32      |   137x137   |  Convolve  |     3x3     |   135x135    |      65x65      |
|      33      |   135x135   |  Convolve  |     3x3     |   133x133    |      67x67      |
|      34      |   133x133   |  Convolve  |     3x3     |   131x131    |      69x69      |
|      35      |   131x131   |  Convolve  |     3x3     |   129x129    |      71x71      |
|      36      |   129x129   |  Convolve  |     3x3     |   127x127    |      73x73      |
|      37      |   127x127   |  Convolve  |     3x3     |   125x125    |      75x75      |
|      38      |   125x125   |  Convolve  |     3x3     |   123x123    |      77x77      |
|      39      |   123x123   |  Convolve  |     3x3     |   121x121    |      79x79      |
|      40      |   121x121   |  Convolve  |     3x3     |   119x119    |      81x81      |
|      41      |   119x119   |  Convolve  |     3x3     |   117x117    |      83x83      |
|      42      |   117x117   |  Convolve  |     3x3     |   115x115    |      85x85      |
|      43      |   115x115   |  Convolve  |     3x3     |   113x113    |      87x87      |
|      44      |   113x113   |  Convolve  |     3x3     |   111x111    |      89x89      |
|      45      |   111x111   |  Convolve  |     3x3     |   109x109    |      91x91      |
|      46      |   109x109   |  Convolve  |     3x3     |   107x107    |      93x93      |
|      47      |   107x107   |  Convolve  |     3x3     |   105x105    |      95x95      |
|      48      |   105x105   |  Convolve  |     3x3     |   103x103    |      97x97      |
|      49      |   103x103   |  Convolve  |     3x3     |   101x101    |      99x99      |
|      50      |   101x101   |  Convolve  |     3x3     |    99x99     |     101x101     |
|      51      |    99x99    |  Convolve  |     3x3     |    97x97     |     103x103     |
|      52      |    97x97    |  Convolve  |     3x3     |    95x95     |     105x105     |
|      53      |    95x95    |  Convolve  |     3x3     |    93x93     |     107x107     |
|      54      |    93x93    |  Convolve  |     3x3     |    91x91     |     109x109     |
|      55      |    91x91    |  Convolve  |     3x3     |    89x89     |     111x111     |
|      56      |    89x89    |  Convolve  |     3x3     |    87x87     |     113x113     |
|      57      |    87x87    |  Convolve  |     3x3     |    85x85     |     115x115     |
|      58      |    85x85    |  Convolve  |     3x3     |    83x83     |     117x117     |
|      59      |    83x83    |  Convolve  |     3x3     |    81x81     |     119x119     |
|      60      |    81x81    |  Convolve  |     3x3     |    79x79     |     121x121     |
|      61      |    79x79    |  Convolve  |     3x3     |    77x77     |     123x123     |
|      62      |    77x77    |  Convolve  |     3x3     |    75x75     |     125x125     |
|      63      |    75x75    |  Convolve  |     3x3     |    73x73     |     127x127     |
|      64      |    73x73    |  Convolve  |     3x3     |    71x71     |     129x129     |
|      65      |    71x71    |  Convolve  |     3x3     |    69x69     |     131x131     |
|      66      |    69x69    |  Convolve  |     3x3     |    67x67     |     133x133     |
|      67      |    67x67    |  Convolve  |     3x3     |    65x65     |     135x135     |
|      68      |    65x65    |  Convolve  |     3x3     |    63x63     |     137x137     |
|      69      |    63x63    |  Convolve  |     3x3     |    61x61     |     139x139     |
|      70      |    61x61    |  Convolve  |     3x3     |    59x59     |     141x141     |
|      71      |    59x59    |  Convolve  |     3x3     |    57x57     |     143x143     |
|      72      |    57x57    |  Convolve  |     3x3     |    55x55     |     145x145     |
|      73      |    55x55    |  Convolve  |     3x3     |    53x53     |     147x147     |
|      74      |    53x53    |  Convolve  |     3x3     |    51x51     |     149x149     |
|      75      |    51x51    |  Convolve  |     3x3     |    49x49     |     151x151     |
|      76      |    49x49    |  Convolve  |     3x3     |    47x47     |     153x153     |
|      77      |    47x47    |  Convolve  |     3x3     |    45x45     |     155x155     |
|      78      |    45x45    |  Convolve  |     3x3     |    43x43     |     157x157     |
|      79      |    43x43    |  Convolve  |     3x3     |    41x41     |     159x159     |
|      80      |    41x41    |  Convolve  |     3x3     |    39x39     |     161x161     |
|      81      |    39x39    |  Convolve  |     3x3     |    37x37     |     163x163     |
|      82      |    37x37    |  Convolve  |     3x3     |    35x35     |     165x165     |
|      83      |    35x35    |  Convolve  |     3x3     |    33x33     |     167x167     |
|      84      |    33x33    |  Convolve  |     3x3     |    31x31     |     169x169     |
|      85      |    31x31    |  Convolve  |     3x3     |    29x29     |     171x171     |
|      86      |    29x29    |  Convolve  |     3x3     |    27x27     |     173x173     |
|      87      |    27x27    |  Convolve  |     3x3     |    25x25     |     175x175     |
|      89      |    25x25    |  Convolve  |     3x3     |    23x23     |     177x177     |
|      90      |    23x23    |  Convolve  |     3x3     |    21x21     |     179x179     |
|      91      |    21x21    |  Convolve  |     3x3     |    19x19     |     181x181     |
|      9       |    19x19    |  Convolve  |     3x3     |    17x17     |     183x183     |
|      93      |    17x17    |  Convolve  |     3x3     |    15x15     |     185x185     |
|      94      |    15x15    |  Convolve  |     3x3     |    13x13     |     187x187     |
|      95      |    13x13    |  Convolve  |     3x3     |    11x11     |     189x189     |
|      96      |    11x11    |  Convolve  |     3x3     |     9x9      |     191x191     |
|      97      |     9x9     |  Convolve  |     3x3     |     7x7      |     193x193     |
|      98      |     7x7     |  Convolve  |     3x3     |     5x5      |     195x195     |
|      99      |     5x5     |  Convolve  |     3x3     |     3x3      |     197x197     |
|     100      |     3x3     |  Convolve  |     3x3     |     1x1      |     199x199     |
