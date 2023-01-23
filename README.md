PhaSER-2C.vi is a LabVIEW application designed to provide real-time phase estimation and initiate bidirectional phase-locked stimulation. LFP signals are acquired using DAQmx functions. The frequency band of interest is filtered using a zero-phase Butterworth topology filter. Forward prediction of this filtered signal is then applied using autoregressive estimation. The phase of this signal is then extracted using a Fast Hilbert transform. Stimulation is triggered on digital output channels when a set target phase is detected.

Requirements:
⦁	LabVIEW2017 or later
⦁	local installation of MATLAB 7.2 or later (for execution of MATLAB script node to estimate autoregressive coefficients using Yule-Walker method in MATLAB)
⦁	DAQmx compatible PCIe daq with at least 5 analog inputs, 2 digital outputs, and one hardware counter

Running:
⦁	Open PhaSER-2C.vi
⦁	Configure front panel settings as appropriate (See following section for detail).
⦁	Click the white 'Run' arrow from the LabVIEW tool bar to run the program.
⦁	Click the 'Acquire Baseline' button on the right-hand side of the PhaSER window to begin recording the LFP signal to train the autoregressive model. The time elapsed will be displayed in the 'Status' display.
⦁	Once 1-2 minutes of baseline LFP has been recorded click the 'STOP' button on the right-hand side of the PhaSER window.
⦁	Update the 'pulse phase' and 'Inhibition Start'/ 'Inhibition End' settings if desired.
⦁	Click the 'Begin Stimulation' button on the right-hand side of the PhaSER window to initiate stimulation. The 'restrict to running?' control can be toggled at any time to enable/disable stimulation outside of running bouts.
⦁	To stop stimulation, click the 'STOP' button on the right-hand side of the PhaSER window.
⦁	Update the 'pulse phase' and 'Inhibition Start'/ 'Inhibition End' settings and click 'Begin Stimulation' if additional stimulation blocks are desired.
⦁	To exit the program, click the red 'Quit' button on the right-hand side of the PhaSER window. Stopping the program using the 'Abort Execution' button in the LabVIEW toolbar is not recommended.


Front Pannel Settings

Analog In Channels:
⦁	Physical Channel: list address for analog input channels for LFP signal, ball tracker signal, and loop in for excitation trigger, excitation pulse train, and inhibition TTL (Dev1/ai0:4 default)
⦁	Min/Max voltage - minimum and maximum values you expect to measure (-1,V 2V default)
⦁	Sample Rate - samples per channel per second (25000 default, see samples to read) 
⦁	Samples to read - number of samples to acquire per channel per read operation (25 default, sample rate and samples to read must maintain a ratio of 1000:1 for subsequent calculations which downsample signal to 1000Hz)
⦁	Channel Assignments - Assigns analog signals to analog input channels 0 through 4. Assigned channels are reflected in 'Channel Number' indicators to the right

Excitation Trigger:
⦁	Counter: Device counter/timer for generating PWM pulses
⦁	Output Terminal: output channel for excitation pulse trigger
⦁	Pulse Phase: excitation target phase in degrees (supports positive and negative values, will be wrapped to a range of [0,360] in calculations)
⦁	Pulse Timeout: minimum period between excitation pulse triggers (100ms by default)
⦁	Pulse Duration: trigger pulse width (0.001 seconds by default)

Inhibition TTL:
⦁	Inhibition Channel: output channel for inhibition TTL signal
⦁	Inhibition Start/ Inhibition End: start and end phase of excitation in degrees for an oscillation with positively increasing phase, i.e. clockwise rotation (supports positive and negative values, will be wrapped to a range of [0,360] in calculations)

Filter Parameters:
⦁	zero phase order: filter order of Butterworth topology bandpass filter
⦁	low cuttoff: lower frequency of pass-band (Hz, 4 by default for theta range)
⦁	high cuttoff: upper frequency of pass-band (Hz, 9 by default for theta range)

Autoregression Parameters:
⦁	width: width of sliding window for filtering in number of samples of 1000Hz downsampled signal (1000 samples or 1000ms by default)
⦁	thickness: width of forward prediction window in number of samples of 1000Hz downsampled signal, i.e. number of milliseconds to predict into the future (150 sample or 150ms by default)
⦁	p: order of the autoregressive model, number of previous samples used to estimate the next sample (13 by default)

Ball Tracker Parameters:
⦁	 Upper/ Lower Speed Bound: signal range from ball tracker (in volts) outside of which the ball is considered to be moving (will need to be calibrated for different movement sensors)
⦁	Sample Length: time window over which average ball movement is calculated (1000ms by default)
⦁	Fractional run threshold: fraction of time that the animal must be running within the window in order to enable stimulation
⦁	restrict to running?: enable/disable stimulation outside of running bouts

Base Filename: File path to write recorded signals to TDMS file
