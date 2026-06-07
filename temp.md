# The why of data-centric AI


Finally here by popular demand: a write-up of the talk I gave recently at PyData London titled _"Models-schmodels: Why you should care about data-centric AI"_. 

Before we start, I'd like to give full credit to my colleagues' excellent blogpost aptly titled "Why you should care about data-centric AI", which served as an inspiration for my talk. Additionally, some credit is due with Rens Dimmendaal for inviting me to participate in the Data-Centric AI competition along with Roel Bertens, which sparked my interest in the topic. 

----

### The way we learn data science
I'm a data science educator at GoDataDriven. That means that I teach courses on all things data science to anybody who's willing to listen. 
< pic of dog > 

Being a data science educator, I got thinking about how we *teach* data science. And more specifically, how I was taught data science. 

I studied Artificial Intelligence at the University of Amsterdam, and everything was very model-focused. We studied the algorithms in detail. Our books were full of equations that gave us a great understanding of the underlying maths, and allowed us to implement the algorithms in Matlab from scratch. I don't even remember what datasets we applied it on, because that didn't matter - it was all about understanding the algorithm. 

< picture of implementation? > 

Then, in order to get more comfortable with the techniques, I practices on Kaggle datasets. You  probably know the Titanic dataset, the most beginner-friendly Kaggle competition where you are led to predict whether passengers are likely survive the Titanic disaster or not based on various characteristics. Not a great example of a data science project that brings value [link?], but a great way to practice your skills. 

Although this was my personal journey of how I learnt data science, it is not an uncommon one. If you look at other ways to learn about data science, like bootcamps and online MOOCs, these often follow the same pattern. Take the Machine Learning course on Coursera for instance: in the list of skills you will learn, it entirely focusses on the algorithms. 

However, when we think of an actual data science project, a common saying is that 80% of the time goes into preparing the data, and only 20% is spend on actual modelling and analysis. So if that's the case.... why do we only teach the modelling part? Why is there such little focus on the practicalities of dealing with our data? 
 
>  Datasets should not be static

[ something something this habit leads to data scientists ]

Data scientists tend to treat their datasets as static, as that is what they learn in most courses and it's what most competitions focus on. It's also almost exclusively the focus of academia, where most papers are on elaborate new algorithms tuned to squeeze a little bit of extra performance on common, *static* benchmarks.  Consequently, it's also what most tools are being built for. 


### The what of data-centric AI 
So this is where *data-centric AI* comes in. Data-centric AI is the discipline of systematically engineering the data used to build an AI system. 

[ it's been gaining traction, Andrew Ng, competition, neurips ]

### The why of data-centric AI 

But a natural question might be - why? Why are we so focussed on the data now, and why is that a new idea? After all, 'garbage in, garbage out' has always been said. Monitoring your data to discover errors has been standard practice for decades, and popular techniques in active learning date back to at least the 90s. So what's different now? 

Well, consider deep learning. Deep learning wasn't a new idea in 201? (when alexnet) -- the first perceptron, the base of the neural network, was described by Rosenblatt as far back as 1958. And backpropagation in 1986. 

But the circumstances have changed. Hardware (GPUs etc.) and large annotated datasets. 

_Even though the idea itself wasn't new, the interest and hype around deep learning certainly was._

So why would that be the case now for data-centric AI? 



### What does this mean for you? 