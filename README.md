# nightwatch-ce
A pre-trained detector you can drop into an inference server, tuned for night footage. Twelve everyday classes, people included, are recognised in stills and clips — the weights were fitted on the publicly available dark-image dataset.

- **Serve it and start detecting**
- **Browse further pre-trained weights**
- **Fit your own**

![](images/1.jpg)

![](images/2.jpg)

![](images/3.jpg)

![](images/4.jpg)



# Serve it and start detecting

The fitted weights recognise these twelve classes in low-light material.

- Bicycle
- Boat
- Bottle
- Bus
- Chair
- Car
- Cat
- Cup
- Dog
- Motorbike
- People
- Table

Steps to get a response out of it:

- **Install the runtime:** follow the vendor's installation notes for the inference server.
- **Fetch the weights:** grab the low-light checkpoint from the release page, then put it in a folder of its own, e.g. a Windows box would end up with the file path `C\Users\MyUser\Documents\Detector-Models\dark.pt`.

- **Start the server:** point it at that folder. An example for a Windows box:

    ```bash
    visionctl --WEIGHT-STORE "C\Users\MyUser\Documents\Detector-Models" --PORT 80
    ```

    And for a Linux host:

    ```bash
    sudo docker run -v /home/MyUser/Documents/Detector-Models:/models -p 80:5000 vision/server
    ```
    A line like the one below shows up in the console once it is serving:

    ![](images/server-log.png)

    From that moment the endpoint `http://localhost:80/v1/vision/custom/dark` answers requests on this host or on `http://your_machine_ip:80/v1/vision/custom/dark`.

- **Send a frame:** POST it to that URL with the `image` parameter filled in. Any language works, or a tool such as POSTMAN. A working script ships in this repository.

    - A still to try it on lives at `images/image.jpg`.


    ![](images/image.jpg)

    - Pull the client library:
        ```bash
        pip install visionclient
        ```
    - Then run the bundled script.
        ```bash
        python detect.py
        ```
    - The script writes an annotated copy to `images/image_detected.jpg` and prints detections in this shape:

        ```
        Name: People
        Confidence: 0.7xx
        x_min: 616
        x_max: 672
        y_min: 224
        y_max: 323
        -----------------------
        Name: Dog
        Confidence: 0.7xx
        x_min: 250
        x_max: 327
        y_min: 288
        y_max: 349
        -----------------------
        Name: Dog
        Confidence: 0.7xx
        x_min: 403
        x_max: 485
        y_min: 283
        y_max: 341
        -----------------------
        Name: Dog
        Confidence: 0.7xx
        x_min: 508
        x_max: 609
        y_min: 309
        y_max: 370
        -----------------------
        Name: Dog
        Confidence: 0.7xx
        x_min: 286
        x_max: 372
        y_min: 316
        y_max: 393
        -----------------------
        ```

        ![](images/image_new.jpg)
    - Other night footage can be pushed through the same call.

# Browse further pre-trained weights

The project keeps a gallery of ready-made checkpoints for the same server on its documentation site.

# Fit your own

To produce weights for your own classes:

- **Collect and label:** gather frames and annotate the shapes you care about, per the dataset-prep notes.
- **Train:** follow the training walkthrough.