# Fundamental of Teachable Machine
Teachable Machine is a web-based tool that makes creating machine learning models fast, easy, and accessible to everyone.Educators, artists, students, innovators, makers of all kinds – really, anyone who has an idea they want to explore. No prerequisite machine learning knowledge required.You train a computer to recognize your images, sounds, and poses without writing any machine learning code. Then, use your model in your own projects, sites, apps, and more.

# Starter Pack
Just go to [teachablemachine.withgoogle.com](https://teachablemachine.withgoogle.com)and click Get Started. No need to make an account or log in. [This playlist of tutorial videos](https://www.youtube.com/playlist?list=PLJfHZtseuscuTQfodmFnbZ3rBgCWsRT9t) are built into the tool to help you along the way. You can also check out [Dan Shiffman’s Coding Train video](https://www.youtube.com/watch?v=kwcillcWOg0&list=PLRqwX-V7Uu6aJwX0rFP-7ccA6ivsPDsK5&index=2&t=0s) on Teachable Machine. Three starter projects for recognizing [fruit](https://medium.com/@warronbebster/teachable-machine-tutorial-bananameter-4bfffa765866), [head tilts](https://medium.com/@warronbebster/teachable-machine-tutorial-head-tilt-f4f6116f491), or [sounds](https://medium.com/@warronbebster/teachable-machine-tutorial-snap-clap-whistle-4212fd7f3555) you make.You can currently train Teachable Machine with images (pulled from your webcam or image files), sounds (in one-second snippets from your mic), and poses (where the computer guesses the position of your arms, legs, etc from an image). More types of training may be coming soon :)

# Getting start with Facial Expression Recognition
Facial expressions convey the emotional state of an individual to observer what is at the back of mind. What we do while we speak often says more than the actual words. There are six basic types of emotions
__1.Happiness 2.Sadness 3.Fear 4.Anger 5.Suprise 6.Disgust.__
![](images/happy1.gif) ![](images/sad1.gif) ![](images/fear1.gif) <br />

![](images/angry1.gif) ![](images/suprise1.gif) ![](images/disgust1.gif)

## How facial expression recognition works?
Facial expression recognition work in 2 parts:
1. Facial Detection:- The ability to detect the location of face in any input image or frame. The output is bounding box of the deteced faces.<br />
2. Emotion Detection:- Classifying the emotion on the face as happy, angry, sad, surprise, disgust or fear.<br />

Learn more about facial expression recognition in [Courcera course](https://www.coursera.org/projects/facial-expression-recognition-keras).<br/>
[Link to DEMO](https://facialexpressionrecognition.yashkalbande.repl.co/) what we are going to build.

## Lets Build the Model 
Vist [teachablemachine.withgoogle.com](https://teachablemachine.withgoogle.com) and clicking on "Get Started". You will see three option to create an image, audio, or pose project. For now, pick “Image Project”.

Click "Add a class" upto 6 classes. Rename "Class 1" to "Happiness" ,"Class 2" to "Sadness", "Class 3" to "Fear", "Class 4" to "Anger", "Class 5" to "Suprise" and "Class 6" to "Disgust"
