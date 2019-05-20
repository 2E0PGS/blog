recently had to calibrate two IC-e208 radios which were around 3KHz off center frequency.

Tools used
* IF output of another radio used as a spectrum scope.
* JIG cable
* SWR and power meter
* Reference transmitter with known good output signal or signal generator
* Dummy load capable of 50W UHF 433.500

Create JIG cable as per diagram here. You will need the resistor and the PTT: http://www.radiomanual.info/schemi/ICOM_VU/IC-208H_serv.pdf

However the resistor should be 22k not 2.2k that is a mistake in the documentation: http://forums.radioreference.com/threads/ic-208h-adjustment-mode-test-jig.153393/

Put the radio into adjustment mode as per the documentation.

Ensure you have dummy load attached to radio.

Ensure the radio has been powered on and warmed up. Give it a bit of TXing to warm it up just before alignment.

Use the JIG cable to engauge PTT. Turn the band dial while its TXing. Mine required a 15-16 setting. See the TX drift across and compare to reference signal on the spectrum graph by alternating TX between the two to align it.

Once aligned press the band button and then hold the power button to turn the radio back off.

Once again verify the output is correct in normal operation mode.
