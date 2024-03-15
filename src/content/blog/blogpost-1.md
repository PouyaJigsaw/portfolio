---
title: Social Avatars for Teleoperation
pubDate: 2023-12-20 14:25
author: "Pouya Pournasir"
tags:
  - Robot Operating System
  - Python
  - User Experience
imgUrl: '../../assets/jackal_mock.jpg'
description: Independently built a large robotic system with more than 4,000 lines of code. Incorporated the Model-View-Controller (MVC) architecture to modularize dialogue and avatar system.
layout: '../../layouts/BlogPost.astro'
---

<iframe width="100%" height="315" src="https://www.youtube.com/embed/obbHqZM5LpA?si=KLdzgnlASYi3wkNb" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# Introduction 
In robot teleoperation, operators and robots collaborate to achieve goals through shared autonomy, reducing operator workload. Trusting the robot is critical for the operator's success, as low trust results in operators not delegating tasks to the robot, even if doing the task themselves creates a high workload that reduces their performance. Research shows trust repair is challenging, but social HRI suggests that robots can rebuild trust in social situations through social strategies such as acknowledging mistakes and promising to do better. This study explores integrating these social strategies and interfaces into teleoperation to enhance trust repair. We compared a social cue-based interface to a conventional one, theorizing that adding in social cues would increase the effectiveness of social trust-repair strategies. Our study found that participants view the social cue-based interface as more capable, a factor that can make the participant put more trust on the system.

## Design Criteria
We designed a scenario where the operator has to control a shared autonomy robot and handle two tasks at the same time. In some cases, one task can be delegated to the robot if the operator trusts the robot’s autonomous capabilities.  
We had to design a video-game like mission for the operator to reach our experiment goals. We inspired from video games like Uncharted or Call of Duty to create a rigid, non-breakable narrative while the non-controllable variables in a robotic mission is much more. We had to engineer each part of the journey so that we are sure about we are analyzing the right data. We also had to engineer the number of mistakes the operator and the robot did in the experiment, at the right place and the right time. The main challenge was to priming them to believe the mistakes we made by themselves, not the creator of the system!
  ![Image 1](src/assets/jackal.png)
  *Two conditions of the experiment: social and non-social. which one increase trust?*

To create our social interface, we created a social agent using human-like language and simple
avatar with animations in comparison to a conventional machine-like terminal. Both represents the
AI (or Agent) that is embedded into the robot and communicates with the user who controls the robot.
We had to create a narrative where: 

1. Trust forms between user and agent (trust formation).
2. the agent breaks trust by performing badly (trust violation). 
3. the agent asks the user about delegating tasks to itself in the future.

The interface was inspired from video games such as Star Fox 64. I used [State Pattern][13] to control each part of the experiment.




<div style="display: flex; justify-content: center; align-items: left;">
    <img src="https://github.com/PouyaJigsaw/teleop-interface/assets/33330581/b231f326-f8bd-4cff-8c81-9c21d73edf17" alt="Image 1" width="50%" height="280"/>
    <img src="https://github.com/PouyaJigsaw/teleop-interface/assets/33330581/1225a286-ca2d-457b-a927-b36c6d9d6822" alt="Image 2" width="50%" height="280"/>
</div>



## Setup
To create a teleoperation system, I used Robot Operating System (ROS) to program a robot called Jackal. 
We connected our robot to a PC and dualshock4 controller, each of them considered a node,using message-passing system of ROS written with python language. 
The controller passes commands to [robot][2], and the robot sends camera data to [PC][3].

[2]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/control/teleop_camera.py
[3]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/camera.py

## Dialogue System (Model-View-Controller)
Just like conventional dialogue-driven video games that shows a dialogue box and an avatar, I
implemented a dialogue system called Avalogue that follows the Model-Control-View (MVC)
pattern.The model is basically two spreadsheets (instead of database) that contains information related to
each dialogue [object][4], and each sprite of animations [such as][5] talking, sad or happy face, etc.
The dialogue is shown word by word, accompanied by a brief sound to mimic old dialogue driven
games (e.g., Star Fox 64). Unlike video games, since our system was real-time, the agent had to
react to different events while talking, so my main challenge was to pause the middle of the agent’s
dialogue, enable the agent to react to the event, and then continue the conversation; something that
is not common in other conventional dialogue systems. 
To fix this, I created a Control class called [Avalogue][6], a composition of [dialogue][7] and [avatar][8]
class I implemented before, with a [deque][9] that contains each Avalogue object. By using Update
loop pattern in the Avalogue class (or Control), new events, which creates new Avalogues, will be
at the top of the stack, and then data will be sent to [dialogue view][10] and [avatar view][11].I tried to
use semaphores to control the flow of dialogues (since each dialogue could be considered a thread)
before switching to this [solution][12].

[4]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/spreadsheets/dialogue_spreadsheet_social.csv
[5]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/spreadsheets/TalkingAvatars.csv
[6]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/avalogue.py
[7]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/dialogue_raw.py
[8]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/avatar_raw.py
[9]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/avalogue.py#L27
[10]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/dialogue.py
[11]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/avatar.py
[12]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/thread_pool.py

## Experiment Design

![Interface](src/assets/interface1.png)
![Interface 2](src/assets/interface2.png)

[13]:
![image](https://github.com/PouyaJigsaw/teleop-interface/assets/33330581/642a8349-ad64-4dfc-b667-8e67cb9a2b82)



## Technical Aspects
For other features, I used other behavioral patterns such as [Singleton][14] and [Observer][15]. Using
baseCanvas as a parent class for numerous UI Elements helped me to re-position or re-size [each
element in general][16]. I wrote a [Shell script][17] which automated the start of each experiment (giving
arguments, opening different scripts, etc.). I also had to do [socket programming][18] to send
messages from my personal laptop to PC, to control parts of the experiment. Version Controlling
using git saved my life, as I added new features (each in its own branch) which broke the system,
so I had to revert to [main branch][19].

[14]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/global_variables.py
[15]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/event.py
[16]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/canvas.py#L165
[17]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/shell.sh
[18]: https://github.com/PouyaJigsaw/teleop-interface/blob/master/src/test/src/main/view/view.py#L464
[19]: https://github.com/PouyaJigsaw/teleop-interface/branches

## Lessons Learned
Making this system single-handedly was a difficult but rewarding experience. Writing
5,000 lines of code with numerous components, while acting simultaneously, made me learn a lot
of Software Engineering concepts. If I had to redo it again, I would not use State Pattern and would
make the system stateless (e.g., using Update loop instead) since for debugging each state, I had
to go through each state one by one to reach the one I wanted. I wish I refactored my code more
(something like a refactor day) and made it easier to debug; at the end of the project the debugging
process was frustrating. Each new feature broke another thing, where I realized the mental toll of
technical debt. But overall, I am proud of this project.
