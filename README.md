# An Open Source Algorithm for the Automatic Labelling of Motion Capture Markers using Deep Learning

An algorithm that uses machine learning to automatically label optical motion capture markers. The algorithm can be trained on existing data or simulated marker trajectories. Data and code is provided to generate the simulated trajectories for custom marker sets.
 

 
 
 # Generate Simulated Trajectories
 If you do not have existing labelled motion capture data to train the algorithm, simulated trajectories can be generated. 
 If you have your own training data, skip this step.
 1. First, the marker set to be used must be defined using an OpenSim marker set .xml file. 
 a) Install OpenSim (https://simtk.org/frs/index.php?group_id=91)
 b) Open model Rajagioa2015_mod.osim included in /data folder
 c) Right click on Markers in the Navigator to add new marker. Marker can be selected and moved in the visualization. Set the marker's name and parent_frame (ie body segment it is attached to) in the Properties window.
 d) Save the marker set by right clicking Markers in the Navigator and choosing Save to File.
 
 2. In generateSimTrajectories.py, set the path to the marker set .xml file. The markers used to align the data also need to be set. Ideally, these will be a marker on the left and right side of the torso or head (eg. right and left acromions).
 3. Run generateSimTrajectories.py
 
 # Train the Algorithm
 1. Set parameters in trainAlgorithm.py.
 a) If using simulated trajectories to train, set the path to the marker set and to the pickle file generated by generateSimTrajectories.py.
 b) If using existing labelled data to train, set the path to the folder containing the c3d files. Set the markers used to align the data. Ideally, these will be a marker on the left and right side of the torso or head (eg. right and left acromions).
 2. Run trainAlgorithm.py. The training will be performed on a GPU, if one is available. 
	Note that this may take a long time to run (ie. hours - days). Training time can be reduced by using less training data (set num_participants in generateSimTrajectories.py).

 # Setup the GUI
 1. Set the paths to trained model, trained values pickle file, and market set in markerLabelGUI.py
 2. Run markerLabelGUI.py, this will open the GUI in your browser.
 
 # Using the GUI 
 1. Enter the folder path where the c3d files to be labelled are located
 2. Select the desired file from the dropdown menu
 3. Click 'Load Data', the loading is complete when the plot of markers appears
 4. If the person is not facing the +x direction, enter the angle to rotate the data about the z-axis (vertical) and click submit. This angle should be chosen such that the person faces +x for the majority of the trial.
 5. Click 'Label Markers'
 6. Clicking and dragging rotates, the scroll wheel zooms, and right clicking translates the plot. Hovering over a marker displays the marker number, label, and coordinates.
 7. The slider at the bottom can be used to view different time frames. After clicking the slider, the left and right arrow keys can be used to adjust the timeframe as well.
 8. The type of marker visualization can be selected from the Visualization dropdown menu. Confidence colour the markers based on the confidence in the predicted label. Unlabelled highlights unlabelled markers in red. Segments displays markers attached to the same body segment in the same colour.
 9. The Error Detection box lists unlabelled markers and duplicated labels. The time frames where the error was detected are displayed. Note that the listed marker is guaranteed to be visible for the first and last frames, but may be missing from the intermediate frames of the listed range.
 10. Labels can be corrected using the Marker Label Modifier. Type the number for the marker to change the label and select the desired label from the dropdown then click the Submit button. To leave a marker unlabelled, leave the dropdown menu blank (this can be cleared by clicking the 'x').
 11. To export a labelled version of the .c3d file, click the Export to C3D button. This will rotate the data back to the original orientation and fill small gaps through cubic spline interpolation. Unlablled markers will not be exported.
 12. Before loading a new c3d file, click the Refresh Setting button.