# AdaRF
Datasets and code used in AdaRF.

## Dataset
There are 5 datasets and each one contains a training data and test data. The detailed description are:

<table class="tg">
  <tr>
    <th class="tg-8jgo" colspan="2">Dataset</th>
    <th class="tg-8jgo">Experiment<br>Times</th>
    <th class="tg-8jgo">Tag<br>Number</th>
    <th class="tg-8jgo">Tag<br>Separation</th>
    <th class="tg-8jgo">Frequency<br>Number</th>
    <th class="tg-8jgo">Sample<br>Number</th>
  </tr>
  <tr>
    <td class="tg-8jgo" rowspan="2">D1</td>
    <td class="tg-8jgo">Train</td>
    <td class="tg-8jgo">250</td>
    <td class="tg-8jgo">5</td>
    <td class="tg-8jgo">20cm</td>
    <td class="tg-8jgo">1</td>
    <td class="tg-8jgo">1250</td>
  </tr>
  <tr>
    <td class="tg-8jgo">Test</td>
    <td class="tg-8jgo">24</td>
    <td class="tg-8jgo">3</td>
    <td class="tg-8jgo">20cm</td>
    <td class="tg-8jgo">8</td>
    <td class="tg-8jgo">72</td>
  </tr>
  <tr>
    <td class="tg-8jgo" rowspan="2">D2</td>
    <td class="tg-8jgo">Train</td>
    <td class="tg-8jgo">40</td>
    <td class="tg-8jgo">6</td>
    <td class="tg-8jgo">8cm</td>
    <td class="tg-8jgo">1</td>
    <td class="tg-8jgo">240</td>
  </tr>
  <tr>
    <td class="tg-8jgo">Test</td>
    <td class="tg-8jgo">16</td>
    <td class="tg-8jgo">8</td>
    <td class="tg-8jgo">8cm</td>
    <td class="tg-8jgo">8</td>
    <td class="tg-8jgo">108</td>
  </tr>
  <tr>
    <td class="tg-8jgo" rowspan="2">D3</td>
    <td class="tg-8jgo">Train</td>
    <td class="tg-8jgo">96</td>
    <td class="tg-8jgo">8</td>
    <td class="tg-8jgo">4-1cm</td>
    <td class="tg-8jgo">1</td>
    <td class="tg-8jgo">768</td>
  </tr>
  <tr>
    <td class="tg-8jgo">Test</td>
    <td class="tg-8jgo">16</td>
    <td class="tg-8jgo">8</td>
    <td class="tg-8jgo">4-10cm</td>
    <td class="tg-8jgo">8</td>
    <td class="tg-8jgo">108</td>
  </tr>
  <tr>
    <td class="tg-8jgo" rowspan="2">D4</td>
    <td class="tg-8jgo">Train</td>
    <td class="tg-8jgo">40</td>
    <td class="tg-8jgo">8</td>
    <td class="tg-8jgo">8cm</td>
    <td class="tg-8jgo">1</td>
    <td class="tg-8jgo">320</td>
  </tr>
  <tr>
    <td class="tg-8jgo">Test</td>
    <td class="tg-8jgo">9</td>
    <td class="tg-8jgo">10</td>
    <td class="tg-8jgo">8cm</td>
    <td class="tg-8jgo">8</td>
    <td class="tg-8jgo">90</td>
  </tr>
  <tr>
    <td class="tg-8jgo" rowspan="2">D5</td>
    <td class="tg-8jgo">Train</td>
    <td class="tg-8jgo">20</td>
    <td class="tg-8jgo">11</td>
    <td class="tg-8jgo">4cm</td>
    <td class="tg-8jgo">1</td>
    <td class="tg-8jgo">220</td>
  </tr>
  <tr>
    <td class="tg-8jgo">Test</td>
    <td class="tg-8jgo">4</td>
    <td class="tg-8jgo">200</td>
    <td class="tg-8jgo">4cm</td>
    <td class="tg-8jgo">8</td>
    <td class="tg-8jgo">80</td>
  </tr>
</table>

### Extract signal profile
In the training data or test data, each folder represent an experiment. There are 50 CSV files in an experiment and each file represent the signal profile collected at an aperture point. In the each CSV file, it consists of the following information:
* EPC: the unique ID of an RFID tag. 24 digits.
* antennaindex: the index of antenna. DEFAULT: 1.
* frequency: the frequency of each item. Range from 920.625 MHz to 924.125 MHz with a step of 0.5 MHz.
* time: INVALID.
* RSSI: a negative real number.
* phase: a real number ranging from 0 to 2pi.
* dopplershift: the dopplershift reported by Impinj reader, which might be inaccurate indicator and is not used in our system.
* velocity: INVALID.
* x: the x-coordinate of the aperture point with the unit of m.
* y: the y-coordinate of the aperture point with the unit of m. DEFAULT: 0. "1.63" is INVALID.
* z: the z-coordinate of the aperture point with the unit of m. DEFAULT: 0.
* angle: INVALID.

At each aperture point, reader can read one RFID tag for many times. Therefore, one tag usually has many phase measurements at a CSV file. To improve the robustness, we use the average of these measurements to represent the phase of the RFID tag collected at the corresponding aperture point.

The corresponding function is available in the file named 'input.py'.

### Reconstruct tag location
The detailed experimental settings are described in 'config.txt', where each line represents the config of an experiment. And each line is a string formatted of "A I:X Y P F S".
* A: date. e.g. "1118" indicates 18 November 2018 while "0114" indicates 14 January 2019.
* I: start index and end index (included). e.g. "1-5" represents 5 experiments from "01" to "05".
* X: the center x-coordinate of the tag sequence with the unit of cm.
* Y: the y-coordinate of the tag sequence with the unit of cm.
* P: the number of aperture points (measurement points). DEFAULT: 50.
* F: the number of frequency (channel). DEFAULT: 8 or 1.
* S: the separation between adjacent tags with the unit of cm. It is an optional item.

In each experiment, antenna scans several RFID tags. All tags have the same y-coordinate and the separation between adjacent tags are same. Therefore, we can reconstruct their locations according to X, Y, S and the number of tags. For instance, if X=50 Y=45 S=20 and the number of tags is 3, the coordinates of these tags are (30, 45), (50, 45) and (70, 45). Note that we only consider locate RFID tag in 2D plane since we only adopt one antenna.

The corresponding function is available in the file named 'config.py'.

### Mapping

A, I, P and S comprose a mapping between the config and folder. For instance, config "1218 1-5:50 45 50 8" maps to the folders from "181218-01-50-8" to "181218-05-50-8".

## Code  (update soon)

