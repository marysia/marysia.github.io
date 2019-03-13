---
layout: post
title:  "Challenge balancing for a kanji e-tutoring system"
subtitle: "a way to improve the learning experience"
date:   2019-02-28 16:25:56 +0100
author: Marysia Winkels
permalink: /blog/kanji-e-tutoring
categories: AI-projects
abstract: "Kanji, the characters in the Japanese writing system, can be notoriously complex and difficult to teach and learn. Maybe even more so than for other domains, learning kanji requires - above all - a lot of repetition, which can be tedious and frankly quite boring. Inspired by an insight from the field of gaming to maximise player engagement, we aimed to improve user engagement with a kanji e-tutoring system by using implicit feedback to gauge the experienced challenge level. The core idea is that we can personalise the experience by adapting our system to provide just the right level of difficulty for each individual user, therefore increasing user engagement and ultimately increasing the time spent using the system."
---
<hr>
### TL;DR
_Kanji, the characters in the Japanese writing system, can be notoriously complex and difficult to teach and learn. Maybe even more so than for other domains, learning kanji requires - above all - a lot of repetition, which can be tedious and frankly quite boring. Inspired by an insight from the field of gaming to maximise player engagement, we aimed to improve user engagement with a kanji e-tutoring system by using implicit feedback to gauge the experienced challenge level. The core idea is that we can personalise the experience by adapting our system to provide just the right level of difficulty for each individual user, therefore increasing user engagement and ultimately increasing the time spent using the system._

<font size="2">
	<b>Note:</b> This project was done as part of the MSc. Artificial Intelligence, under supervision of <a href="http://www.roijers.info">Diederik Roijers</a> and in collaboration with the Japanese language department of Leiden University. This work was presented at <a href="https://bnaic2018.nl)">BNAIC 2018</a>.
</font>
<hr>

<i><emph> Kanji</emph></i> are the logographic characters borrowed from Chinese used to write Japanese, where each character corresponds to a content word. Kanji can be complex, as they may consist of smaller components (radicals) and can be used in conjunction with other kanji to form new meanings. For example, the combination of the kanji for 'electricity' and the kanji for 'car' means 'train'. The number of kanji characters ranges in the thousands, with a little over 2000 being in on the joyo list: the list of kanji designated for daily use.

<div class="Figure">
    <img src="../assets/kanji/compound_kanji.png" alt="Image" width="200"/>
	<br>
	<div class="Figure index">Figure 1.</div><div class="Figure description"> Example of compound kanji. </div>
</div>
 
Maybe even more so than in other domains, kanji requires individual practise and repetition to master, more than a classroom approach, which is why <emph>e-tutoring systems</emph> are often used to supplement - or even replace - lectures, by providing instructions, asking questions and providing feedback. The aim for such a system is to keep the student as engaged as possible, in order to prevent the student abandoning the system out of frustration, and to facilitate the necessary repetition to master the writing system. 

One way to do this is by what we call a _competence model_, which includes parameters for the difficulty of the individual skills to acquire (_e.g._ per character), as well as the current level with respect to each skill for each student. A major limitation, however, is that this tends to be highly data-intensive, even in small domains. To combat this, we exploit the key observation that it is often not necessary to explicitly model competence in order to provide the appropriate challenge to the student. Instead, we can learn  a mapping from <emph>implicit user feedback</emph> to <emph>experienced challenge level</emph>, and use this to provide the appropriate next learning object (such as question or example) for each student individually. 



# Kanji e-tutoring system
Our e-tutoring system provides the user with a series of <emph>multiple choice</emph> questions where they have to provide either the respective kanji or meaning of the kanji. In addition to this, we provide a 'hint' option which can guide the student in the right direction, for example by outlining from what radicals or individual kanji a kanji exists, or a mnemonic.

<div class="Figure">
    <img src="../assets/kanji/mountain.png" alt="Image" width="300"/>
	<br>
	<div class="Figure index">Figure 2.</div><div class="Figure description"> Hint presented for the kanji for mountain. </div>
</div>

Because of the way we set up the system and the nature of kanji themselves, we can more or less estimate how difficult a question will be for a student in relative terms. For example, when we present the kanji for trees and provide the option trees, forest, plants and rice field, we expect the question to be more difficult than if we presented car, woman and mountain as alternative options. <emph>Difficulty vector v</emph> is the combination of parameters that determine the relative difficulty, and we've asked an expert from the Japanese language department of the University of Leiden to rank these parameters in order of difficulty. 

# Offline learning & online adaptation
Now we've defined our e-tutoring environment, but we want to make it <emph>adaptive</emph> to the user. To do that, we need to create a model that maps implicit user feedback to the perceived challenge level in the <emph>offline learning</emph> phase. This means we need to collect data in order to determine what the perceived challenge level is. We did this by presenting users with chunks of questions generated from the same difficulty vector, and asking them what their perceived challenge level was on a 1-5 scale, ranging from way too easy to way too hard. We record the implicit user feedback, such as whether they answered the questions correctly, how long they took and whether they requested hints. 


We both trained a model solely on correctness, and one on the correctness in combination of the implicit user feedback. The model trained with implicit user feedback was _more effective_ in determining the perceived challenge level than training only on correctness.

In the <emph>online adaptation</emph> phase, we use the mapping we created in the e-tutoring environment. A user answers a chunk of questions, and based on the implicit feedback, we estimate how close this chunk was to the right challenge level. Then, using this information, we jump in the ranking of difficulty vectors to a new difficulty vector and generate the new chunk of questions from that. This continues for the duration of the use of the program.

# Results
So, of course, we want to know whether our system is actually preferred by students. We test this by presenting users with our system and a baseline system in random order, for a set number of questions. They then had to indicate which system they preferred, and why. Further feedback included users explicitly noting that the adaptive system better matched their skill level.
 
<div class="Figure">
	<img src="../assets/kanji/results.png" alt="Image" width="800"/> 
	<div class="Figure index">Figure 3.</div><div class="Figure description"> Preference by users for the adaptive system vs. baseline system. </div>
</div>


Initial results already confirmed that using implicit user feedback provides a better mapping to perceived challenge level than solely looking at the correctness of the answers, and further testing indicate that users generally prefer a system based on this method. We can therefore conclude that challenge balancing based on implicit user feedback provides a <emph>viable alternative</emph> to competence models based challenge balancing.
