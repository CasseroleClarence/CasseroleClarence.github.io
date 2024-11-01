# Biomedical Training Device

## Background and Overview
Lower Mainland Biomedical Engineering (LMBME) faces a growing demand for effective and flexible training of Biomedical Engineering Technologists (BMETs), who are responsible for maintaining over 110,000 medical devices across multiple hospitals. Current training options, such as factory training and in-house sessions, present significant challenges. Factory training is costly and limited in availability, while in-house training faces scheduling difficulties, limited equipment, and space constraints. As BMETs require up-to-date knowledge to maintain hospital devices properly, inadequate training could negatively impact hospital operations, patient care, and potentially increase healthcare costs.

The economic and operational consequences of insufficient training are substantial. Poorly maintained devices lead to frequent repairs, increased maintenance costs, and reduced hospital efficiency. Compromised patient care due to malfunctioning devices can also result in legal consequences, as per regulations like the Hospital Act of BC. This issue is critical not just for LMBME but also for the broader healthcare system, as malfunctioning devices can disrupt essential services and delay patient diagnostics.

This leads us to the aim of this project. To address the challenges of BMET training, we explored several alternative methods, including augmented reality (AR) simulations and online learning programs, in hopes of finding more cost-effective and flexible solutions. However, each of these methods encountered its own set of issues. For instance, while online options reduce costs and alleviate scheduling constraints, they often lack the hands-on interaction that is crucial for effective BMET training. In contrast, augmented reality faces challenges related to cost, as multiple headsets are required to accommodate several trainees, and the medical devices needed for training are frequently in use at hospitals. Consequently, we propose our solution: a training machine that uses a projector to display an interactive image onto a workbench. This approach combines the interactivity of traditional and augmented reality training while being significantly more cost-effective and accessible for widespread use.

This page discusses a Biomedical Engineering Technologist training device that was developed for BMEG 357 (Biomedical Engineering Design II). As the goal of the class is to mainly perform research on the problem at hand to determine the best solution, the final deliverable of this class is a report of our research and a critical function prototype, a barebones proof of concept. I will first be discussing the actual prototype we ended up with as well as my work that went into it, but if you are at any time interested in the research behind our solution [you can click here](#research). 

## Critical Function Prototype - The Holotrainer

### Development considerations
When developing our prototype, we had 2 main factors to consider, projection and sensing. We needed a way to project the image and training instructions on our work surface and a way to sense that the user was interacting object projected. The 2 biggest sources of inspiration for our work are laser keyboards that are projected on your desk to be used normally - which was honestly just a novelty item, and those interactive floor projectors that malls will sometimes have for kids that project things like bubbles or fish that would move around if you stepped on them. 

To address our method of projection, we did first come across laser keyboards and wanted to base our design off of that. However, we found a little too late that unless you either 1. have a system of motors to move the laser diode around and outline the image, or 2. pass the beam through a specialized template like in traditional laser keyboards, you would not able to make complex shapes such as an infusion pump that BMETs would be training to use, and this isn't even taking into account the fact that the latter option only gives you a static image, making it unable to display the various training steps or different views of the machine. This led us move away from laser projection and towards an LCD projector based device for our solution proposal, more similar to the afforementioned interactive floor projection. This would enable us to display multiple moving images as needed during the training. 

After figuring out how we wanted to project our images, we move onto considering how our device will sense user input. Traditionally, interactive floor projections detect users using a wide variety of sensors and cameras, such as infrared and pressure sensors, as well as depth cameras - like how a Microsoft Kinect works. However, these solutions were all a combination of either being too expensive to buy, too complicated to develop with our prototyping budget or would just not work with our intended application, so we elected to look into how laser keyboards detect user input. To summarize, laser keyboards use an infrared camera mounted high and pointing down, as well as an infrared line laser mounted low, which projects a plane of infrared light near and parallel to the table's surface. As the user touches the table to interact, their finger will reflect infrared light to the camera, which then informs the program where the user's finger is in the camera feed. This allows the device to differentiate between intentional inputs (touching the surface) versus accidental inputs which could come from hovering their hand above the image.

Now with those details fleshed out - we could now actually start developing!

### Implementation
To reiterate, as this is a critical function prototype, our developed prototype only encompasses our indentified critical function, which we found to be how the devices sense input. 

#### Hardware 
The hardware for this project is relatively simple, all we needed was an infrared camera, a line laser, a projector, and a stand to put everything on. All of these things are pretty easy to find online and order, so not much work needs to be done here. 

[add screenshot of budget]

Oh right... our budget. As this is a class project, our budget was a measly 100 CAD. So, what can we do? Well we can do what we do best as engineers and be resourceful as well as make some swaps. Rather than a projector, as we're just trying to demonstrate our device's sensing function, we can swap that our for 2 laser pointers that will project 2 circles on the surface, which can act as "buttons", and rather than an infrared camera, we can instead remove the infrared filter from a cheap webcam. 

#### Software
The software for this project was written entirely by me in Python. It was honestly a great experience since I had learned some basic Python in the past but never really got a chance to apply it until now. The software revolves almost entirely around the OpenCV library, which contains various computer vision tools that I needed to learn and understand to use effectively.

The code can be summarized as a series of filtering and processing steps that culminate in the program being able to output the coordinates of a user's finger in the camera feed. It opens the camera stream and defines two small rectangular regions on the frame, which serve as reference points for detecting whether the finger is inside them, these points are hardcoded in as the stand which holds the lasers pointers and camera will be fixed in place, so we can get away with it.

The main loop begins by capturing each frame from the camera. It draws the rectangles on the frame for visualization and converts the frame from the BGR color space to HSV (Hue, Saturation, Value). This makes our input frame much more suitable for the value/brightness based filter we will be using as our input is infrared light - which through the camera without an infrared filter looks white. The code then enters a calibration phase, which requires the user to place a finger on each dot on the table (inside the defined rectangles), the code then takes the HSV values from the pixels in the rectangle to define an approximate range the program should be looking for when trying to identify user input. The code then isolates a specific color range in the HSV space—corresponding to the calibration results and creates a simple threshold mask that highlights only the areas matching this color and then converts the result to greyscale, leaving us with a grey image with our regions of interest (fingers touch table) highlighted. 

After this, several image processing techniques are applied, the first of which is a combination of gaussian blurring using a 5x5 kernal to smooth out the finger contours and reduce any noise in the background of the image and otsu's binarization which is another thresholding function to further refine the highlighted frame. 
> Otsu's method automatically determines the optimal threshold value to separate the foreground from the background in a greyscaled image using an algorithm that was created by much smarter people. This step is important after blurring the image because Gaussian blurring smooths out noise and small variations, but the image may still contain uneven intensities or unclear edges. Otsu’s thresholding is applied on the blurred grayscale image to produce a clean, binary image where the foreground (finger) is clearly separated from the background. In short, the two thresholding operations work in tandem: the first filters by color, while Otsu's thresholding ensures clean, noise-free contours for more accurate detection. 

Following this, I applied morphological operations (opening followed by closing) to clean up any remaining noise in the background and close up any small holes in our foreground contour to ensure that only significant regions remain in the binary image. Once this filtering is complete, the program detects contours within the image and for each detected contour, the code calculates the minimum-area bounding box and computes its centroid using image moments. 

The centroid represents the center of the detected contour, and the program checks if the centroid lies within either of the predefined rectangles. If the centroid is inside one of the rectangles, the program changes the rectangle's color to red, indicating that the finger has entered that area. This dynamic system allows the program to track the finger in real-time, displaying its coordinates and giving visual feedback about its position. 

Currently, the program is set up to walk the user through 2 steps, to touch the red dot and subsequently the green dot, and to respond accordingly if the instruction is followed correctly or not. 

Overall, this project was a hands-on exploration of computer vision techniques using Python and OpenCV. It allowed me to dive deep into image processing, color filtering, and contour detection while creating an interactive system that tracks and responds to user input through a camera feed. 



## <a id="research"></a> Research 


# Mixed Reality Training for Biomedical Technologists

## Background and Overview
This project addresses the challenge of providing efficient, cost-effective, and accessible training for Biomedical Engineering Technologists (BMETs). The Lower Mainland Biomedical Engineering (LMBME) group faces high costs, limited scheduling, and equipment shortages in current training models. Our goal is to develop a mixed reality (MR) training method to enable BMETs to perform critical device maintenance efficiently.

## 1.0 Needs Assessment
- **Medical Problem**: Discusses the demand for BMET training, existing limitations in training options, and the impact of inadequate training on device reliability and patient care.
- **Scope Definition**: LMBME’s needs include training for the BD Alaris infusion system, budget constraints, limited equipment, and scheduling restrictions.
- **Current Solution Limitations**: The one-time, eight-hour training model lacks flexibility and fails to build confidence in BMETs over the long term.

## 2.0 Alternative Training Models
Explores several training models, each evaluated for strengths and limitations:
- **Factory Training**: Expensive and inflexible but comprehensive.
- **In-House Training**: More accessible but limited in device availability.
- **Online Simulations and Video Training**: Cost-effective but lack hands-on experience.
- **Blended Learning**: Combines online and in-person training for greater flexibility.

## 3.0 Stakeholder Analysis
- **Technicians**: Require effective training to maintain devices confidently.
- **Hospital Administrators**: Focus on sustainability and cost-efficiency.
- **Patients**: Depend on reliable, well-maintained equipment for quality care.
- **Additional Perspectives**: Insights into BMET, hospital management, and clinician concerns regarding VR/AR training.

## 4.0 Needs and Specifications
### Needs Summary
Defines target users, markets, and the potential for MR solutions to address the training gaps in healthcare institutions. Key considerations include real-time feedback, cost-effectiveness, and scalable infrastructure.

### Specifications and Requirements
Details the functional and operational requirements, including:
- **Interactive Feedback**: Real-time instructional feedback to simulate in-person training.
- **Cost-Effective Implementation**: Aims for lower costs than current training models.
- **User-Friendly Design**: Prioritizes accessibility for BMETs of varying tech proficiencies.

## 5.0 Concept Development
- **Concept Generation**: Eight initial MR training models (e.g., VR headsets, mobile apps, wearable sensors).
- **Concept Screening and Ranking**: Ranked based on plausibility, feasibility, and scalability, leading to the selection of the top concepts.

## 6.0 Final Design Concept
- **Virtual Projected PCU**: The final design concept uses a virtual PCU with sensors and projectors to simulate maintenance procedures. It eliminates the need for physical devices, reducing costs and setup time.

## 7.0 Evaluation Criteria and Scoring
Explains how concepts were scored and weighted, with factors such as:
- **Scheduling Needs**: How self-directed each training method is.
- **Cost**: Comparative analysis of five-year costs.
- **Ease of Setup**: Evaluation of initial setup and maintenance.

## 8.0 Contributions
Lists individual team contributions, covering needs identification, stakeholder analysis, concept scoring, and final presentation.

---


