# Anamoly Detection on Tenmperature Control Data
Anomaly detection using data from the TCLab with on/off heater control method

In this example, we’ll be able to generate some real data with the Temperate Control Lab device and train a supervised classifier to detect anomalies. The TCLab is a great little device for generating real data with a simple plug-and-play Arduino device. If you want to create your own data for this, there are great introductory resources found [here](http://apmonitor.com/pdc/index.php/Main/ArduinoTemperatureControl).

## Problem Framework
The TCLab is a simple Arduino device with two heaters and two temperature sensors. It’s a simple plug and play device, with a plug to power the heaters and a USB port to communicate with the computer. The heater level can be adjusted in a Python script (be sure to pip install tclab), and the temperature sensor is used to read the temperature surrounding each heater. For this example, we’ll keep it basic and just use one heater and sensor, but the same principles could also be applied to the 2-heater system, or even more complex systems such as what you might find in a chemical refinery.

We can imagine the problem as this: we have a heater for our garage workshop, with a simple on/off setting. We can program the heater to turn on or off for certain amounts of time each day to keep the temperature at a comfortable level. There are, of course, more sophisticated control systems we could use; however, this is designed as an introduction to anomaly detection with machine learning, so we’ll keep the raw data simple for now.

Under normal circumstances, it’s a simple enough exercise to verify that the heater is on and doing its job — just look at the temperature and see if it’s going up. 
But what if there are external factors that complicate the evaluation? 
- Maybe the garage door was left open and lets a draft in.
- some of your equipment starts overheating. 
- if there’s a cyberattack on the temperature control, and it’s masked by the attackers? 

Is there a way to look at the data we’re gathering and determine when something is going wrong? This is the heart of anomaly detection.

With a supervised classifier, we can look at the sensor data and train it to classify when the heater is on or off. Since we also know when the heater should be on or off, we can then apply the classifier to any new data coming in and determine whether the behavior lines up with the data we see. If the two don’t match, we know there’s an anomaly of some type, and can investigate further.

## Data Preprocessing
One of the keys to machine learning is investigating how to frame your data in a way that is useful for the model. When I do any project in machine learning, this is often the most time-consuming step. If I get it right, the model works like a charm; otherwise, I can spend hours or even days of frustration, wondering why my model won’t work.


### Train Evaluation
![Training Result](images\train.png)


The answer to if we can just use the raw temperature as the input is an emphatic no. This makes intuitive sense — for example, at 35°C, the heater is either on or off. However, the change in temperature would tell us a whole lot about what the heater is doing.

### Anomaly Detection with the Trained Classifier
Here is when the problem hits. We know the classifier works well to tell us when the heater is on or off. Can it tell us when something unexpected is happening?
The process is really the same. The only difference is now we have an anomaly in the data. We’ll create the new feature (change in temperature, using the smoothed rolling average), scale the data, and feed it into the classifier to predict.


![Anomaly Result](images\anomaly.png)

This looks like it worked well! We see the prediction is that the heater is off when the fan is running, when in reality the heater is on, there’s just an anomalous event. 





**Anomaly Detection - Temperature Control Data.ipynb**: Integrates with the TCLab to generate the test.csv and train.csv files used for the anomaly detection exercise

**Generate Data.ipynb**: Exercise using data from the TCLab to train and test a supervised classifier for anomaly detection.

**test.csv**: data file containing test (anomalous) data

**train.csv**: data file containing train (clean) data
