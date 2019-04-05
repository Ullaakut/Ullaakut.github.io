---
title: "How refactoring my C++ application into a Go library made it better in every way"
published: true
---

<p align="center">
    <img width="100%" src="https://cdn-images-1.medium.com/max/2000/1*Uy1i5ZbvY0OWPWCrmjMxQg.png">
    <span class="figcaption_hack"><i>982 lines added, 20 309 lines removed — original PR</i> <a href="https://github.com/Ullaakut/cameradar/pull/78">here</a></span>
</p>

# How refactoring my C++ application into a Go library made it better in every way

*This is my own experience, and these results were mainly achieved because Golang is a perfect language for my specific project, while C++ is not.*

I began working on the [Cameradar](https://github.com/Ullaakut/cameradar) project in March 2016. The idea started out in an internal meeting at work as a quick shell script intended to automatically discover cameras in order to reduce the time it takes to configure our CCTV system in new data centers. I decided to make it in C++ since that was the language we used primarily in our tech stack and it was the language I had the most experience in.

The original version had many features:

* Camera discovery
* Dictionary attacks
* Stream preview generation
* Stream quality checking
* Runtime loading of cache managers (using dynamic libraries)

<br/>

My former team leader, [Jérémy Letang](https://medium.com/@letang.jeremy), told me that I could make it open source and let other people use it, which was really exciting for me as an intern at that time. I became the maintainer of my first open source project!

As time went by, the project shifted more and more **from a developer tool to a user-friendly command line tool **due to my focus on making it an easy tool for anyone to use. Integrating it into a micro-service at work came with technical debt because of the tool’s flawed design. It was good as a docker image to use for quick penetration testing or to discover the credentials of your brand-new undocumented Chinese camera, but bad for its original intent.

*****

[Julien Bordellier](https://medium.com/u/558cdffaae6c) then proposed **making it a library**, which would make it much easier to integrate into our micro-service, and was compatible with keeping a user-friendly tool for the current users.

I decided to strip out the features that were initially part of Cameradar in order to fit the internal business needs of the tool but were useless for most users. The thumbnail generation, stream checks and the possibility to connect Cameradar to a database were no longer needed if it became a library. Our internal micro-service or even another tool could now handle this part of the logic, that had nothing to do with the discovery and access of camera streams.

I got to work. I was extremely surprised to have a working library and binary after only 2 and a half hours of work, despite my very basic knowledge of Go! There were bugs around and improvements to be done, but it was discovering and accessing cameras successfully. The binary basically replaces the old application and also provides an example of how to use the library’s features.

A few statistics of the code base at that time:

* Reduced the number of lines of code by **95%**
* Reduced the docker image size by **85%**
* Added unit tests in **under 2 hours**
* Made the CI much simpler and **90% faster**

<br/>

As well as other interesting facts:

* Documentation is now automatically generated via [godoc](https://godoc.org/github.com/Ullaakut/cameradar)
* The dependency management provided by [glide](https://github.com/Masterminds/glide) makes my life easier (UPDATE: It now uses dep and will soon switch to go modules 🤷🏽‍)
* The user experience is much better than before thanks to a few awesome libraries
* **Adding new features to Cameradar has never been so simple and fast**

<br/>

<p align="center">
    <img width="100%" src="https://cdn-images-1.medium.com/max/1600/1*x58UbQPEnwvezho4gSfRLQ.gif"/>
    <span class="figcaption_hack"><i>the output of the app is now simple and clean</i></span>
</p>

Of course it would have been *possible* to code unit tests for the C++ version, and also *possible* to give it a nice output like showed in the image below, but it would have taken much more maintenance and time than in Go, and in the case of my project it would have been for no benefit at all.

It’s been only a few weeks since the 2.0.0 version is out, but I found again the early excitement I had for working on this project, and many more people are now using it, so I’m really happy about this decision. I can’t wait to refactor the C++ micro-service that I made a year ago using the C++ Cameradar, into a Go micro-service.

I’m not the only one to report such results:

> _“I have reimplemented a networking project from Scala to Go. Scala code is 6000 lines. Go is about 3000. Even though Go does not have the power of abbreviation, the flexible type system seems to out-run Scala when the programs start getting longer. Hence, Go produces much shorter code asymptotically.”_
**—Petar Maymounko**

And not only is it shorter to write than most languages, but it’s also often easier to code and to test:

> _“I’ve spent more time coding than debugging and it’s so simple, fast and funny…”_
— **Roberto Costumero**

A final argument that made this code refactoring worthwhile in my opinion, is that the Go community has been very supportive and is actively encouraging people of every level to contribute, which is wonderful for a beginner like me.

#### To conclude, I would recommend to anyone who struggles to maintain a repository that feels like legacy and is full of technical debt, to give Go a try if it seems to fit your needs. You’d be surprised by how fast you would see results.

I hope you enjoyed this article, feel free to [contact me](https://www.linkedin.com/in/brendanleglaunec/) if you want to discuss the content of this article, or if you’re considering refactoring your project, I would be happy to talk about it with you.

### [Brendan Le Glaunec](https://medium.com/@brendanleglaunec)

Software Engineer [@Containous](http://twitter.com/Containous) <br/> [https://github.com/Ullaakut](https://github.com/Ullaakut) <br/> [http://ullaakut.eu](http://ullaakut.eu/)
