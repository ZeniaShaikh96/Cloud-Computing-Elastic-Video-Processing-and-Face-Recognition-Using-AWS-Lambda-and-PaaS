# Cloud-Computing-Elastic-Video-Processing-and-Face-Recognition-Using-AWS-Lambda-and-PaaS
Built an elastic, cost-effective PaaS app using AWS Lambda for scalable, serverless video analysis with advanced cloud programming techniques.

Detailed Step-by-Step Project Description
This project involves building a scalable, serverless video analysis application on AWS using PaaS. It processes user-uploaded videos to extract frames, detects faces, and identifies individuals, leveraging AWS Lambda and S3.

1. Overview and Objectives
Goal: Create a scalable and cost-effective video processing pipeline using AWS PaaS services.
Key Features:
Automatically scales out and in based on workload.
Processes video uploads to perform frame extraction and face recognition.
Stores results in well-structured S3 buckets for retrieval.

2. Core Components
2.1 AWS S3 Buckets
Three S3 buckets are required:

Input Bucket: <ASU ID>-input

Stores uploaded .mp4 video files.
Triggers the video-splitting Lambda function upon upload.
Stage-1 Bucket: <ASU ID>-stage-1

Stores extracted frames from the videos.
Each frame is saved with the same name as the video but with a .jpg extension.
Output Bucket: <ASU ID>-output

Stores the final face recognition results.
Each text file corresponds to a video, containing the name of the identified individual.
2.2 AWS Lambda Functions
Two Lambda functions handle the video processing:

Video-Splitting Function:

Name: video-splitting
Triggered by video uploads to the input bucket.
Process:
Extracts a single frame from the video using FFmpeg.
Saves the frame to the stage-1 bucket as <video_name>.jpg.

Face-Recognition Function:

Name: face-recognition
Triggered by the completion of the video-splitting function.
Process:
Takes the frame and detects faces using OpenCV APIs.
Computes face embeddings using a ResNet-34 model.
Compares embeddings with a pre-trained dataset (data.pt) to identify faces.
Stores the identified name in a text file in the output bucket.

3. Application Workflow
Step 1: Video Upload
Users upload .mp4 videos to the input bucket.
The upload triggers the video-splitting Lambda function.
Step 2: Frame Extraction (Video-Splitting Lambda Function)
The video-splitting function processes each video:
Extracts one frame from the video using FFmpeg.
Saves the frame in the stage-1 bucket.
Step 3: Face Detection and Recognition (Face-Recognition Lambda Function)
The face-recognition function processes each frame:
Detects faces using OpenCV APIs.
Computes embeddings using a ResNet-34 model.
Matches embeddings with a dataset (data.pt) to identify individuals.
Saves the result in a text file in the output bucket.
Step 4: Result Storage
The output bucket contains .txt files where each file corresponds to a video.
Each file contains the name of the identified individual.

4. Testing and Validation
4.1 Test Cases
Bucket Validation:
Ensure the input, stage-1, and output buckets exist with proper naming.
Confirm buckets are empty initially.
Lambda Function Validation:
Verify the existence and proper configuration of the video-splitting and face-recognition Lambda functions.
Pipeline Execution:
Test the entire pipeline with 100 videos.
Validate the following:
Stage-1 bucket contains 100 frames (.jpg files).
Output bucket contains 100 text files (.txt files) with correct face recognition results.
4.2 Performance Metrics
Latency: Ensure end-to-end processing time for 100 videos is ≤ 300 seconds.
Concurrency: Validate the Lambda functions can handle multiple invocations simultaneously.

5. Scalability and Optimization
Serverless Architecture: Uses AWS Lambda to automatically scale based on workload.
Cost Efficiency: Avoids over-provisioning of resources by processing tasks on demand.
Optimization Techniques:
Use lightweight Docker containers or AWS-provided runtimes for Lambda functions.
Reduce PUT requests by extracting only one frame per video.
