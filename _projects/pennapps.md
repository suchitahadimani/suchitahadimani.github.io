---
layout: page
title: Khaki AI
description: Close your eyes and dance, we'll make the music for you!
img: assets/img/khakiai.jpg
importance: 2
category: hackathon
related_publications: true
---

### PennApps XXV

Devpost: [https://devpost.com/software/khakiai](https://devpost.com/software/khakiai )

I had a lot of fun developing Khaki AI and this was my first hackathon win :))) 

Here is the rough timeline of how we developed Khaki:

##### Friday before the opening ceremony —- 

Sahil and I got to check-in pretty early around 3pm. After checking in and getting our merch, we were mostly sitting around and brainstorming ideas. We were thinking of some real world problems that we could solve (global warming, poverty, the typical). Based on previous hackathons, we noticed that most winners aimed to solve actual problems. But all the ideas we came up with felt very lifeless and forced. 

Sahil and I met Aakash through one of Sahil’s friends. The three of us went to starbucks to get a coffee. I got the apple crisp oatmilk macchiato which didn’t taste as bad as I thought it would, considering I don’t like oatmilk very much.

As we got our Starbucks, we continued brainstorming ideas. All three of us really wanted to do computer vision projects because it just seemed really fun. I had never worked on CV before and wanted to try playing around with it. We also didn’t want to do a serious topic because it was draining our energy and we were in the mood for something more light. My proposed idea for the project was to determine the percentage of jellyfish a person would be based on some random set of criteria. The user would open their camera app and be immersed in the world of jellyfishes (jellyfishi?). The PennApps theme was deep sea, so I thought it was a very fitting topic and I was aiming for the most useless hack.

But that app idea wouldn’t be very fun/interactive/impactful. Sahil suggested using Suno AI to create music to go along with dance videos. It was a simple idea and was fun, too.

Honestly, I was skeptical about using Suno AI as we had used it before at HackMIT and had spent a lot of time trying to make it work, with no success. Their product is absolutely amazing and generates really good music with original lyrics. It does far better than most of its competitors in terms of lyrics, vocals, variety of genres supported, and originality. 


But we decided to go for it anyway.


##### Friday evening —

After the opening ceremony, we ate food, did some basic system design, figured out which tracks we wanted to apply for, and bumped into Leo, our 4th member. We were all wearing khaki pants when we met, so we decided to call our product KhakiAI.

We began programming around 10pm. For the roboflow track, we wanted to use their software to train a model for the dances. We quickly realized the plan would require a lot of data collection and annotation, which might be fun but also not worth the effort with a hackathon time crunch. Instead, we found a model called OpenPose which essentially gives the real time x and y coordinates for key joints (shoulder, elbow, knee, etc). 

We followed this tutorial to use the model in our project: [https://blog.roboflow.com/what-is-openpose/](https://blog.roboflow.com/what-is-openpose/) .

Along with the pose information, we also extracted the time information and stored it in a json file as such (there are a lot of null values as this recording was mainly recording me from waist up):

```json
{
    "timestamp": {
        "$numberDouble": "0.03510093688964844"
      },
      "keypoints": {
        "Nose": {
          "x": { "$numberInt": "612" },
          "y": { "$numberInt": "406" },
          "confidence": {
            "$numberDouble": "0.3682267665863037"
          }
        },
        "Neck": {
          "x": { "$numberInt": "612" },
          "y": { "$numberInt": "579" },
          "confidence": {
            "$numberDouble": "0.49259644746780396"
          }
        },
        "RShoulder": {
          "x": { "$numberInt": "361" },
          "y": { "$numberInt": "579" },
          "confidence": {
            "$numberDouble": "0.4844030439853668"
          }
        },
        "RElbow": {
          "x": { "$numberInt": "751" },
          "y": { "$numberInt": "344" },
          "confidence": {
            "$numberDouble": "0.3392183184623718"
          }
        },
        "RWrist": {
          "x": { "$numberInt": "723" },
          "y": { "$numberInt": "406" },
          "confidence": {
            "$numberDouble": "0.2627209424972534"
          }
        },
        "LShoulder": {
          "x": { "$numberInt": "834" },
          "y": { "$numberInt": "563" },
          "confidence": {
            "$numberDouble": "0.49720337986946106"
          }
        },
        "LElbow": null,
        "LWrist": null,
        "RHip": null,
        "RKnee": null,
        "RAnkle": null,
        "LHip": null,
        "LKnee": null,
        "LAnkle": null,
        "REye": null,
        "LEye": null,
        "REar": null,
        "LEar": null,
        "Background": {
          "x": { "$numberInt": "27" },
          "y": { "$numberInt": "673" },
          "confidence": {
            "$numberDouble": "1.0090644359588623"
          }
        }
      },
      "image_width": { "$numberInt": "1280" },
      "image_height": { "$numberInt": "720" }
    }

````



While we wanted to do real time music generation as a person danced, that idea seemed too hectic, especially without SunoAI fully figured out and the complexities of changing the music in real time. We decided to have the videos recorded first and then create the music based on the entire video.

To store the video recordings and the json files, we decided to use mongodb. We changed our python file to follow the flask template to make it easier to do calls to the database.

Next we worked on using the json information to extract the music predictions. We weren’t sure whether to train a model for this or not, but we tried plugging it into ChatGPT just to see what would happen. GPT gave some pretty epic results (it would highlight at which time stamps to put the beats on and everything) so we decided to use an LLM to generate a music prompt. We wanted to be eligible for the Cerebras and Tune prizes. So, we used the llama 3.1 8b model provided by Cerebras and then deployed that on Tune studio. 

We had to do some prompt engineering to figure out how to generate prompts to feed to Suno. It would sometimes give too much detail or explain reasoning. E.g. “this music is hip hop because it would best match the pose data which shows the user is moving their hand quickly” which was not something we could feed to Suno.

 This was the following prompt we gave to llama: 

> "Analyze the provided position coordinates and timestamps from the pose data. Do NOT explain your thought process or give a filler introduction. Generate a concise prompt for a song, specifying the genre, topic, and beats per minute. Use the following coordinates data and if coordinates of body parts are changing more with shorter time intervals, the song should tend happier. If the coordinates change less, it should tend sadder. Give one complete sentence! {json_data}"



Based on the question above, we would receive prompts like this:

> “Write an upbeat dance song in the genre of electronic dance music (EDM) at 128 beats per minute, titled \"Vibrant Movement \" with lyrics that capture the dynamic and energetic changes in body movements."


We also spent some time figuring out Suno AI. Having launched less than a year ago, they have yet to develop a proper API documentation as their platform is mainly user-centric, and not for developers. We were able to figure out how to make the API calls using cookies, but ran out of credits before we could test more.

We slept around 4 AM.

##### Saturday ---

Woke up around 10:30ish and got breakfast at 11 but we were still hungry so we went to Starbucks. We bumped into Naman from Tune AI there who gave us the news that Tune would natively support Cerebras which was crazy to hear (and probably meant everyone had done the same thing and used Cerebras through Tune).

Probably started actually coding around 1 pm. While the guys were busy figuring out Suno AI (kudos to them, because I had given up on it), I worked on creating a UI for our application as everything was running in the terminal at this stage. I used Next.js to create the frontend for our flask backend. It just had two pages, one to record the video, and the second to display the final video. 

I tried to integrate PropelAuth into our application to be eligible for the sponsor prize, but I was tired and simply couldn’t figure out how to integrate it. Now, I will forever be biased and prefer Clerk for authentication (set it up in like 10 minutes at HackMIT).

I also tried to deploy to Vercel, but I had the video files download first and then play on our website, which I think caused issues with deployment? Either way gave up on that too.

Grabbed dinner and slept early around 8-9pm. While I slept, the guys worked on Suno api, the devpost, presentation, systems design drawing, and the logo. 

I woke up around 4 am. They had figured out how to make audio files from Suno to download. I spent the next couple hours figuring out how to integrate their backend files with the pre-existing files. I ended up using local files to facilitate communication between the next-flask files and the suno files (aka saving the prompt generated with the Flask backend to a local file and then having the Suno backend reading from that).

Finished all our coding work at 7 AM and recorded our demo video by 9 and called it a hackathon.

[Demo Video](https://youtu.be/hOJUfDrPAnw?feature=shared)



##### Sunday —-

There was a huge delay with the start of judging but we became friends with the neighboring groups in the judging room. Judging itself went pretty well. All the judges had a blast dancing and getting their custom songs.

Went to grab lunch at this Indian restaurant which had food way too spicy for any of us.

##### Closing Ceremony —

Won Most Creative Hack – will be getting an amazon echo pop :)) 


##### System Design:

{% include figure.liquid loading="eager" path="assets/img/khaki_sysdes.png" class="img-fluid rounded z-depth-1" %}




