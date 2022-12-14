# Research Report
## Introducition
Music is an integral aspect of countless lives, and as such it is often woven into debates relating to ethics, copyright, and censorship, among others. It is no secret that much of the most popular songs today are marked as explicit—and thought explicit music is generally not allowed on public radio, the rise in CDs, MP3s, and streaming has allowed explicit music to thrive. While the implications of the prevalence of explicit music will not be discussed, the focus of this project is to examine better ways of analyzing and flagging explicit music.

Currently, music labels and streaming services use a binary system for classifying explicit music (i.e. a track is either clean or explicit). While this system works in a pinch for DJs and parents to quickly nix certain tracks, it does not take a rocket scientist to see that “WAP” by Cardi B is much more offensive than “Oxford Comma” by Vampire Weekend, despite both being labeled as explicit. In fact this system often fails significantly. Some songs slip past the explicit stamp because they cleverly disguised offensive content in ambiguous sexual innuendos, while other relatively harmless songs get flagged because of a couple moderately offensive words. This is because the means of marking the songs—and the system as a whole—is somewhat arbitrary. Thus it would be of great benefit to devise a system that flags offensive songs with more subtlety and nuance than a simple yes-or-no seal.

The desire for offensive language detection and classification is not a rare one either. Debates and conversations online are rife with foul language, which many platforms are not a fan of. It would be impossible to manually look at each and every message and flag any offensive language. Thus, companies turn to computers to analyze language for them. These systems are still imperfect, which makes this research project a good contribution to the overall work being done to categorize human language. The same problems that exist in these online forums also apply to music. There are over 70 million tracks on Spotify alone, and it only takes a few minutes for an independent artist to upload their songs to an online platform. The sheer volume of data and democratization of music dispersal should be cause for analysis, especially given the problems in the current classification system.

This is, however, easier said than done. Utilizing artificial intelligence to flag human speech is bound to be imperfect. Natural Language Processing systems still struggle heavily with ambiguity—something that only becomes more complicated with music. Song lyrics tend to be artful and abstract, and often play with double meanings—especially when it comes to offensive tracks that are trying to still get airtime. This also leans into a philosophical debate about meaning—what exactly makes a song offensive or not? Is it something inherent or is it constructed by the listener?

Thus, utilizing AI will not provide a perfect solution to the problems outlined. Even a completely human system would not be able to do that. The broader aim of this investigation is to see how NLP can be used to analyze questions like these, and to encourage those investigating language today to rethink the methods we used to classify speech.

My research project focused solely on the text of the song. While music can convey tone and meaning through sounds other than words, the text of a track will still generally contain the most information about a song’s content (plus, it would be next to impossible to try and incorporate other elements of a song into this project). As this area of language analysis is near the forefront of NLP research, much of my work is supported by and drawn on previous research. Much work has already been done analyzing trends in music marked as explicit using the traditional explicitness-marking system, as well as incorporating NLP into flagging offensive text. However, these AI systems generally still use a binary system. My aim was to investigate how NLP can be used to generate a more nuanced system for analyzing offensive text.

## Methods and Results

Initially, I planned on creating my own AI program to read in a song’s lyrics and return a scaled rating of a song’s explicit content. However, two main principles kept me from doing so. One, machine learning is a new area for me, and I feared that spending weeks learning, designing, and trying to perfect a new system would prevent me from seeing whether this approach would work. Instead, my own design might be flawed and thus not provide a good answer as to whether AI can be used for qualifying explicit language. Secondly, I did not want to reinvent the wheel. As this area of NLP is filled with research, there was no need to redesign something that might already exist. Therefore, after spending several days researching machine learning systems, looking at previous work on offensive language detection systems, reading articles on the implications of the prevalence of offensive language, toying with NLP systems, and thinking about how I would design my own system, I ultimately decided to rework elements of this previous research.

As much of the previous work on offensive language uses a binary system, or is done internally by private companies, I actually feared there would be little to work from. When I looked into popular NLP libraries, I found packages that would usually tell you the tone of a snippet of text…not exactly the finer-grained analysis I desired. However, I found a Python library that might be suitable for the job. Victor Zhao published an article in 2019 detailing his work creating an accurate offensive language detection system (https://towardsdatascience.com/building-a-better-profanity-detection-library-with-scikit-learn-3638b2f2c4c2). His machine learning program was trained on data from Wikipedia and Twitter (i.e. not song lyrics), so it would likely struggle more with song lyrics. While his approach still uses a binary system, he included a function that provided the probability that a given snippet of text would be explicit or not. Initially skeptical, I soon believed that this probability measure could actually give me the analysis I was looking for. Intuitively, a piece of text that is very apparently offensive and filled with foul language would have a high probability of being flagged as offensive. Likewise, a piece of text that is very ordinary and polite will have a low probability of being explicit. Furthermore, the binary explicit label is derived from the probability rating (i.e. text with a probability above 50% will be marked explicit). My initial tests with these functions seemed to support my theories. Importantly, lines of text with only a few light curse words landed somewhere in the middle.

I decided to run with these package and run tests on a dataset of songs to determine its accuracy. The GitHub page for Victor Zhao’s project ended up being deprecated, but I found a separate page that coded an updated version of the library (https://gitlab.com/dimitrios/alt-profanity-check). I located a dataset created for a project by Amy Tan, which includes around 3000 songs, their lyrics, and Spotify’s explicit label (https://medium.com/@tanyufei0514/nlp-detecting-explicit-content-in-music-lyrics-7cd0450779e5). This dataset is itself taken from a larger Kaggle dataset of about 57,000 songs and modified to include the explicit labels. After running the probability and explicit estimation functions on all the lyrics, I found that this program generally aligned generally with Spotify’s own explicit labels. The model aligned with Spotify’s labels 86.06% of the time, with clean songs being marked more accurately (91.08%) than explicit songs (71.23%). Though not a perfect correspondence, several of the mismatched can be attributed to either errors with Spotify’s labeling (explicit songs that were not marked as such) or more ambiguous cases (eg. songs labeled explicit for only a few swear words). In this initial, smaller dataset, the average probability of a song being explicit (i.e. its explicitness rating) was 31.78. For songs already labeled explicit, the average rating was 71.69 (out of 100). When the inaccurate songs are factored out, the rating goes up to 90.40. For songs not labeled explicit, the average rating was 18.26 and 13.00 when the mismatched songs were factored out. Interestingly, when looking at the mismatched songs in isolation, they seemed to better resemble scores from the opposite category. Tracks that Spotify labelled as clean, but the program marked as explicit had an average score of 71.98 (compared to the average score of 71.23 for Spotify’s explicit songs). Likewise, explicit songs the program marked as clean had an average score of 25.35 (compared with the average score of 18.26 for Spotify’s clean songs).

I plotted the distribution of scores on a histogram to see if there was anything of note. On the lower end of the spectrum, there seems to be a wider range of scores that the program will output, though the scores are still bundled closer to the 00.00-10.00 range.
![](hist_cln.png)
With explicit songs, though, the divide is much stronger. Almost all songs are in the range of 95.00-100.00, which indicates that the program is not necessarily giving as fine-grained an analysis as I had hoped.
![](hist_exp.png)
For the most part, it seems to be confident that a song is explicit, with a good number of songs still landing somewhere in-between. Looking to my example of two songs that are both technically explicit, but vary in offensive content, they both received similar scores (99.99 for “WAP” and 95.79 for “Oxford Comma”). While “WAP” rightfully came out with a worse score, I expected “Oxford Comma” to fare better. An AI program more tailored to song data—and designed to better differentiate offensive language—may provide a different result. However, what speech is more offensive than others also leads to a slippery—and often subjective—slope.

With these results in mind I decided to analyze a much larger dataset. I found a dataset on Kaggle of about 15000 songs, mostly centered around 6 genres (edm, latin, pop, r&b, rap, and rock) and spanning from 1957-2020 (https://www.kaggle.com/datasets/imuhammad/audio-features-and-lyrics-of-spotify-songs). Ideally, the dataset would include more songs from more genres and time periods, but most smaller-scale music datasets do not include the track’s lyrics. In all, the average explicitness rating was 34.60, with explicit songs scoring 85.62 and clean songs scoring 16.15 on average. From there, I wanted to analyze the explicitness ratings and percentages by genre and decade.

The dataset I used broke down every song into a broad genre and a subgenre. In all, there were 6 genres and 24 subgenres, while the song release dates spanned from 1957 to 2020.
![](genre_overall.png)
![](genre_split.png)
After running the functions on the entire dataset, I found that, unsurprisingly, rap had the highest average explicitness rating as well as the greatest percentage of explicit songs. Meanwhile, EDM had the lowest rating and percentage.
![](subgenre_overall.png)
![](subgenre_split.png)
Among the 24 subgenres, gangster rap came out on top, followed closely by hip hop and southern hip hop. All three also had the greatest percentage of explicit tracks. Another dance genre, progressive electro house, had the lowest score, followed by tropical. The two also had the lowest percentage of explicit songs. In general, as was the goal of this project, there was more of a gradient of explicitness scores across genres. For example, a genre like reggaeton, seemed to straddle the line of explicit and clean, with an average track rating of 52.31. Interestingly, however, the percentage of explicit tracks seemed to correlate strongly with the average explicitness rating. Due to this correlation, it might be possible to determine relative offensiveness of a whole genre by looking at the frequency of its explicit tracks, rather than use a rating of the genre’s explicitness.

By decade I noticed some other unsurprising trends.
![](decade_overall.png)
![](decade_split.png)
The 1980s saw a sharp increase in explicitness ratings as well as frequency of explicit tracks. Both figures declined slightly for each progressive decade. Relatedly, the relative explicitness of songs of the 80s and on was much higher than in previous decades. Meaning, of the songs labeled explicit, each song generally had a higher explicitness score than songs from the 60s and 70s. This indicates that not only were explicit songs more frequent, but they were also more offensive from the 80s on.

## Final Thoughts

A lot more went into the research process than is shown on this page, but I wanted to present a condensed form that highlights the key points. The analysis here is more of a proof-of-concept that using Artificial Intelligence to analyze explicit music quite viable. Furthermore, analyzing music using a more fine-grained rating has benefits that a binary system does not have.

Going forward, continued research in this area of offensive language has immense benefits that spread beyond music. While I employed a preexisting language library for the current analysis, another language classification system can be designed that is more tailored for song lyrics. Despite that, I may continue to run with this system and design an application that can give a line-by-line analysis of a given song to indicate where exactly the most and least offensive parts lie. Broadly speaking, there is a lot that can be made from this starting point—be it applications for music or research in another area of Natural Language Processing.

## References
Below is a list of all the resources I used to aid my research.
### Data and Datasets
* https://www.kaggle.com/datasets/imuhammad/audio-features-and-lyrics-of-spotify-songs
* https://michaeljohns.github.io/lyrics-lab
* https://github.com/micbuffa/WasabiDataset
* http://millionsongdataset.com/

### Offensive language and AI
* https://github.com/rominf/profanity-filter
* https://pypi.org/project/profanity-filter
* https://medium.com/@tanyufei0514/nlp-detecting-explicit-content-in-music-lyrics-7cd0450779e5
* https://towardsdatascience.com/building-a-better-profanity-detection-library-with-scikit-learn-3638b2f2c4c2
* https://gitlab.com/dimitrios/alt-profanity-check
* https://www.textrics.ai/solutions/offensive-language-detection
* https://github.com/t-davidson/hate-speech-and-offensive-language
* https://www.surgehq.ai/blog/the-obscenity-list-surge
* https://www.wired.com/story/ai-list-dirty-naughty-obscene-bad-words/
* https://imusician.pro/en/support/faqs/article/what-is-considered-as-explicit-content
* https://www.scientificamerican.com/article/can-ai-identify-toxic-online-content/
* https://www.semanticscholar.org/paper/An-Analysis-of-Profanity-in-English-Lyrics-Amdan-Shaari/91c4d64f5b80c226d3f59c90480f01ea1bf094b1
* http://www.ijstr.org/final-print/feb2020/and-I-Swear-Profanity-In-Pop-Music-Lyrics-On-The-American-Billboard-Charts-2009-2018-And-The-Effect-On-Youtube-Popularity.pdf
* [Misogyny in Rap Music: A Content Analysis of Prevalence and Meanings](https://webfiles.uci.edu/ckubrin/Misogyny%20in%20Rap%20Music.pdf?uniq=fn1t7r)
* [Undressing the Words: Prevalence of Profanity, Misogyny, Violence, and Gender Role References in Popular Music from 2006-2016](https://www.researchgate.net/publication/304538918_Undressing_the_Words_Prevalence_of_Profanity_Misogyny_Violence_and_Gender_Role_References_in_Popular_Music)
* [Explicit Content Detection in Music Lyrics Using Machine Learning](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8367165&tag=1)
* [A Study of Profanity Effect in Sentiment Analysis on Natural Language Processing Using ANN](https://journals.riverpublishers.com/index.php/JWE/article/view/12727)
* [Neural Models for Offensive Language Detection](https://arxiv.org/pdf/2106.14609.pdf)
* [Explicit song lyrics detection with subword-enriched word embeddings](https://www.sciencedirect.com/science/article/abs/pii/S095741742030573X)

### Python NLP programs
* https://towardsdatascience.com/real-time-sentiment-analysis-on-social-media-with-open-source-tools-f864ca239afe
* https://towardsdatascience.com/become-a-lyrical-genius-4362e7710e43
* https://www.cio.com/article/189218/what-is-sentiment-analysis-using-nlp-and-ml-to-extract-meaning.html
* https://spacy.io/
* https://conversationai.github.io/
* https://www.datacamp.com/tutorial/R-nlp-machine-learning
* https://towardsdatascience.com/topic-modelling-in-python-with-spacy-and-gensim-dc8f7748bdbf
* https://towardsdatascience.com/how-to-analyze-emotions-and-words-of-the-lyrics-from-your-favorite-music-artist-bbca10411283
* [Using machine learning analysis to interpret the relationship between music emotion and lyric features](https://peerj.com/articles/cs-785/)
* [Lyrical Analysis Using Machine Learning Algorithm](https://www.irjet.net/archives/V5/i4/IRJET-V5I4578.pdf)
* [Deep learning techniques for rating prediction: a survey of the state-of-the-art](https://link.springer.com/article/10.1007/s10462-020-09892-9)
