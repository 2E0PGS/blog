# Bristol LoRa

I ordered two of these in the 868 MHz variant from AliExpress: [LILYGO® TTGO Disaster-Radio LoRa32 V2.1 1.6 Version 433/868/915MHZ LoRa ESP-32 OLED 0.96 Inch SD Card Bluetooth WIFI Module](https://www.aliexpress.com/item/4000396836096.html)

They come pre flashed with the disaster.radio firmware.

Just did some range tests and had really impressive results.

I suggested some improvements to the project. Below is a couple of my suggestions/bug reports along with related links:

* [Routing table error after long uptime or 3+ nodes #38](https://github.com/sudomesh/disaster-radio/issues/38)
* [disaster-radio-website/issues/4](https://github.com/sudomesh/disaster-radio-website/issues/4)
* [868 Mhz setting for Europe #43](https://github.com/sudomesh/disaster-radio/issues/43) Sam raised the ticket but this is something we discussed in the "Bristol LoRa" WhatsApp group.
	* [adds ability to configure lora frequency and spreading factor via con…](https://github.com/sudomesh/disaster-radio/commit/a94ca40b72bf207a8d4c7fca0d11128dada49459)
	* [add lora frequency as config option](https://github.com/sudomesh/disaster-radio/commit/93aafa36f6aff5abe6fe12e06d3e831ce188059e)
	* [expose LoRaFrequency and spreadingFactor in public functions](https://github.com/sudomesh/LoRaLayer2/commit/8509821d2a04b5edccaaaab7a6ef3260aa7b2cba)
* [releases/tag/0.2.0](https://github.com/sudomesh/disaster-radio/releases/tag/0.2.0)