<hr>
<h1 style="text-align:center">Exploring Edge Mining for Context-Aware Ubiquitous Learning using Smart Watches</h1>
<h4 style="text-align:center">PI: <em> Imran Zualkernan</em></h4>
<h4 style="text-align:center">Award: <em>EFRG18-SCR-CEN-82</em> </h4>
<img src="https://upload.wikimedia.org/wikipedia/en/c/c8/American_University_of_Sharjah_%28emblem%29.png" style="margin-left:auto;margin-right: auto;display: block" >
<!-- <img src="http://auscse.com/main/images/banner.png" style="float: right;" > -->
<hr>
<h2>Steps to setup the Experiment</h1>
<ol>
    <li>
        <h3>Basic Setup Steps</h3>
        <ul>
            <li>Download the zip file and extract it locally.</li>
            <li>Move the project to your <em>/home</em> directory.</li>
            <li>Switch to the project folder in command line and enter <strong>npm install</strong>.</li>
            <li>That should install the required dependencies of <em>mqtt, cassandra-driver, exceljs and pcap2csv</em> ,
                otherwise manually install them.</li>
            <li>Open <strong>testAutomater.js</strong> file and change the <em>server_file</em> and <em>pcaps</em>
                constants to point to your exact <strong>server.js</strong> file as shown in the default value and also
                to your <strong>pcaps</strong> directory where the throughput.csv file will be saved.</li>
            <li>Install Cassandra DB on your machine. Steps can be found at this link
                http://cassandra.apache.org/download/</li>
            <li>
                <p>After installation, an instance/node of cassandra should be running automatically, check status by
                    running
                    <strong>nodetool status</strong> or <strong>sudo service cassandra status</strong>. Both should give
                    you an active or running status for one node. </p>
                <p>Note: If you get nodetool error or service cassandra status returns that the service has exited, it
                    means that cassandra is crashing, and probably has to do with the amount of space that is required.
                    Try increasing your VM RAM to about 16 GB or so and retry.</p>
            </li>
            <li>Additionally, you will need an MQTT broker running locally, try using PONTE npm broker. Installation and
                setup steps can be found at https://www.npmjs.com/package/ponte</li>
            <li>Once the environment is setup and the broker is running, start the server.</li>
            <li>Change the experiment number by modifying the <em>exp_num</em> variable if you run the experiment more
                than once. This will create a new record/table in the DB for every experiment you run.</li>
            <li>The server displays all the data it receives from the watch and also saves it in the DB with a unique
                key called <em>timestamp</em>.</li>
            <li>You can view the entries in the DB by running <strong>cqlsh</strong> on command line and entering the
                Cassandra Query Language(CQL) shell. From there just enter <strong>select * from
                    watch_analytics.experiment#</strong> with your experiment # and view the results.</li>
            <li>Change the <em>tcconfigprofiles</em> variable to your downloaded project directory and
                <strong>tcconfigprofiles</strong> directory under it </li>
            <li>Change the <em>network_driver</em> variable to your wireless adapter name, when you run <em>ip addr</em>
                or <em>ifconfig</em>.</li>
            <li>Similarly change the <em>pcaps</em> variable to your pcaps directory location in your downloaded
                project.</li>
        </ul>
    </li>
    <li>
        <h3>Setup for Tizen OS (Samsung Gear S3)</h3>
        <p>There are 3 use cases for Samsung Watches, namely:-
            <ol>
                <li>Sensor Data, local and offloaded processing</li>
                <li>Tensorflow Audio Model inference on watch vs. offloaded</li>
                <li>Tensorflow Toxicity Text Model inference on watch vs. offloaded</li>
            </ol>
        </p>
        <ol>
            <li>
                <h4>Sensor Data</h4>
                <ul>
                    <li>Download the project from https://github.com/AUS-uni/Samsung-Tizen-WatchApp-Sensors </li>
                    <li>If running a Samsung watch, change the <em>targetWatch</em> constant in the <em>testAutomater.js</em>
                        file to the address of your watch
                        too.</li>
                    <li>Make sure the test machine and the watch are on the same network.</li>
                    <li>Make sure that the MQTT broker IP Address is configured both on the dowloaded watch project under <em>js</em> -> <em>models</em> -> <strong><em>heartRate.js</em></strong>   </li>
                    <li>This is also the file with all of the logic for the app.</li>
                    <li>On the server project, start the <strong>sensor_offload.js</strong> file if you are running an example with sensor offloading. The rest of the configuration can be done on the excel file as mentioend in the server automation setup steps.</li>
                    <li>Sometimes, the first run may not work because of SDB related issues, so re run it again.</li>
                </ul>
            </li>
            <li>
                <h4>Audio Model</h4>
                <ul>
                    <li>Download the project from https://github.com/AUS-uni/Samsung-Audio-TFModel-WatchApp</li>
                    <li>
                        Fill in the Examples.xlsx to match your test criteria. The main variables to set in this file are -
                        <ul>
                          <li>Network Profile</li>
                          <li>Duration</li>
                          <li>test_type -> choose between <strong>audio</strong> and <strong>c_audio</strong> only</li>
                          <li>No need to set sensor here</li>
                          <li>pick whether you want periodic/poisson frequency for sending data</li>
                          <li>Based on what you picked above set the <strong>period</strong> or <strong>poisson_frequency</strong></li>
                          <li>Set the <strong>model</strong> to 1 which implies the audio TF model.</li>
                        </ul>
                      </li>
                      <li>
                          If you are doing offloading experiment with Audio model you will need to start the <strong>getAudiowatch.js</strong> file and the
                          <strong>audiobrowser.html</strong> files too before starting the experiment. Run these files separately on a terminal
                          using <em>node getAudiowatch.js</em> and double cliking on the <strong>audiobrowser.html</strong> file.
                          The script will start the node script and the html file will open on your browser and ask for microphone permssions which you will have to allow.
                          The script listens for incoming audio blobs from the watch, then creates local wav files from the watch audio blob.
                          It will then notify the browser to prepare for listening and then plays the audio file for the browser to listen.
                          The browser then listens, predicts and sends the result back to the watch.
                      </li>
                      <li>If you are doing local experiment for Audio, then you do not need to start anything else, everything will happen within the watch
                          program.
                      </li>
                      <li>Finally, run testAutomator.js to start the automated experiment</li>
                </ul>
            </li>
            <li>
                <h4>Toxicity Model</h4>
                <ul>
                    <li>Download the project from https://github.com/AUS-uni/smartwatch-toxicity-model </li>
                    <li>
                        Fill in the Examples.xlsx to match your test criteria. The main variables to set in this file are -
                        <ul>
                          <li>Network Profile</li>
                          <li>Duration</li>
                          <li>test_type -> choose between <strong>text_local</strong> and <strong>text_offload</strong> only</li>
                          <li>No need to set sensor here</li>
                          <li>pick whether you want periodic/poisson frequency for sending data</li>
                          <li>Based on what you picked above set the *period* or <strong>poisson_frequency</strong></li>
                          <li>Set the <strong>model</strong> to 2 which implies the text model.</li>
                        </ul>
                      </li>
                      <li>
                          If you are doing offloading experiment with Toxicity model you will need to start the <strong>sensor_offload.js</strong> file too before
                          starting the experiment. Run this file separately on a terminal using <em>node sensor_offload.js</em>. This script listens for incoming
                          input words or sentences and then classifies them as toxic or not and sends them back via MQTT to the watch program.
                      </li>
                      <li>If you are doing local experiment for Toxicity, then you do not need to start anything else, everything will happen within the watch
                          program.
                      </li>
                      <li>Finally, run testAutomator.js to start the automated experiment</li>
                </ul>
            </li>
        </ol>
    </li>
    <li>
        <h3>Setup for Android Wear (Huawei Watch)</h3>
        <ul>
            <li>Download the Nativescript Android App from https://github.com/AUS-uni/Android-Nativescript-WatchApp.
            </li>
            <li>Move the downloaded project to the same location as your Main Server (this project). </li>
            <li>Go to your watch and enable adb debugging under developer options.</li>
            <li>Make sure the test machine and the watch are on the same network.</li>
            <li>Also enable Wi-fi debugging or debugging over Wi-fi which will give you an address to connect to along
                with the port number, usually 5555 by default.</li>
            <li> change the <em>adb_huawei_addr</em> variable to the IP address of your watch with the port number as
                the default value shows.</li>
            <li>Lastly, install the Nativescript CLI globally by running <em>npm install -g nativescript</em>.</li>
        </ul>
    </li>
    <li>
        <h3>Setup for Fitbit OS Device (Fitbit Versa)</h3>
        <ul>
            <li>Download the Fitbit Device Fitbit App from https://github.com/AUS-uni/Fitbit-Sensors-WatchApp</li>
            <li>Move the downloaded project to the same location as your Main Server (this project).</li>
            <li>Download the Fitbit App on your phone which is the primary pair of the Fitbit device.</li>
            <li>Enable bluetooth and Wifi on your phone and open the app.</li>
            <li>Select your device and enable <em>developer bridge</em> from the developer options.</li>
            <li>On your Fitbit device go to settings and enable the Developer Bridge too.</li>
            <li>Make sure the test machine, the watch and the companion (The phone which is paired with your Fitbit
                using the Fitbit App) are on the same network.</li>
            <li>When running the server and having a Fitbit device, a second shell will open as
                <strong>fitbit$</strong>, enter the command <em>install</em> here.</li>
        </ul>
    </li>
    <li>
        <h3>Setup for Apple Watch and iPhone (Project built using Watch Series 3 and iPhone 5s)</h3>
        <ul>
            <li>Download the Apple Project from https://github.com/AUS-uni/Apple-WatchApp-SensorData</li>
            <li>Make sure you have downloaded the project on an Apple Computer with the latest XCode Editor.</li>
            <li>Ensure all other developer permissions are enabled on your XCode and your devices. Refer to Apple Developer Guides for necesary licenses and developer requirements.</li>
            <li>The <strong>SessionHandler.swift</strong> file under the <em>watchtest</em> directory is where the main logic for MQTT connection and sending sensor data lies.</li>
            <li>Modify <strong>mqttConfig</strong> variable in this file to match <strong>host</strong> parameter with the IP Address of your MQTT Broker.</li>
            <li>Under the <strong>override init()</strong> function, all of the MQTT Callback methods can be overridden with your use case, but do not change it if not needed.</li>
            <li>The <strong>func session()</strong> method is where the companion App listens for the watch to send its accelerometer data with coordinates and then envelopes it into an MQTT message by stringifying it and sending it to the main server.</li>
            <li>The above can be modified to receive other sensor data from the watch to the companion, <strong>*However in this case of Apple, heartrate data or any health related data is under the HealthKit library of Apple and requires long and unconnected methods to retrieve them therefore they are ommitted from this watch's use case. You can use all Motion related sensors like Gyroscope, Accelerometer etc. Another thing to note is that the method of launching the app is different from all other watches too because of the uniqueness of the way Apple development works (this will be mentioned in the next point).</strong></li>
            <li>Proceed to launch the <em>testAutomater.js</em> file with the steps under <strong>Steps to Automate the Experiment</strong> and under <strong>Network Shaping & Requirements</strong> sections.</li>
            <li>In this exception, the <em>testAutomator.js</em> will not start the watch app on your Apple Watch, so after beginning the server, launch the app on your watch using XCode by building and running the project. Select Apple Watch under devices.</li>
            <li>Once the app is launched on the watch and the phone, the watch will automatically keep sending it's data to the server without the server having multiple callbacks.</li>
            <li><strong>*Note: This watch has unique approaches and use cases because of the stark differences in the way Apple Development behaves as compared to Android, Google or the Web.</strong></li>
        </ul>
    </li>
    <li>
        <h3>Setup for Arduino Device (ESP 32)</h3>
        <ul>
            <li>Change the SSID, password and IP to the one in which you are running your experiment.</li>
            <li>download and install arduino IDE from https://www.arduino.cc/en/Main/Software</li>
            <li>Run arduino IDE</li>
            <li>go to <em>file-> preferences</em>  and paste <strong>https://dl.espressif.com/dl/package_esp32_index.json</strong> in additional
                boards manager url field</li>
            <li>go to <em>tools->board->board manager</em> and search for <strong>esp32</strong> and install</li>
            <li>go to <em>tools->manage libraries</em>, and search for <strong>mqtt</strong>.</li>
            <li>select and install MQTT by <strong><em>joel gaehwiler</em></strong></li>
            <li>copy the code from <strong>ESPMQTTArduinoTestAuto.c</strong> to arduino ide, verify and upload</li>
        </ul>
    </li>
    <li>
        <h3>Network Shaping Setup & Requirements</h3>
        <ul>
            <li> you will need to install <strong>tcconfig</strong>. </li>
            <li>On debian/ubuntu enter <strong>wget
                    https://github.com/thombashi/tcconfig/releases/download/v0.19.0/tcconfig_0.19.0_amd64.deb</strong>
            </li>
            <li>Follow by this command <strong>sudo dpkg -i tcconfig_0.19.0_amd64.deb</strong></li>
            <li>You will also need to install Wireshark to use the tshark command line utility, this is simply done from
                the <em>Ubuntu Software</em>.</li>
            <li>If you get the following error, <em>couldn't run /usr/bin/dumpcap in child process: Permission
                    Denied</em> or something similar its a permissions related issue and add your user to the wireshark
                group by-
                <ol>
                    <li>sudo dpkg-reconfigure wireshark-common</li>
                    <li>choose answer as "YES" .Then add user to the group by</li>
                    <li>sudo adduser $USER wireshark</li>
                    <li>Restart your machine or wireshark.</li>
                    <li>The pcaps you capture should not be locked, a useful link-
                        https://askubuntu.com/questions/458762/how-to-enable-wireshark-without-running-as-root-in-trusty-14-04
                    </li>
                </ol>
            </li>
            <li>After successfull run, in the <em>pcaps</em> directory, you should get a <strong>throughput.csv</strong>
                file which has the network
                related information. For every re-run of the same experiment, remove the old pcaps and csv file. For new
                experiments, change <em>exp_num</em> and the <strong>Examples.xlsx</strong> file.</li>
        </ul>
    </li>
    <li>
        <h3>Steps to Automate the Experiment</h3>
        <ul>
            <li> <strong>Examples.xlsx</strong> contains information to automate the tests. </li>
            <li>Change columns <em>B, F, G, H and I</em> to the values you want and save the file.</li>
            <li>Check if any device has a new IP that is not already configured in the 7 json profiles in
                tcconfigprofiles directory, then,
                <ol>
                    <li>Open <strong>addnewip.js</strong> file and change <em>new_ip</em> variable to the new IP that
                        you want to add. </li>
                    <li>Run the file using <em>node addnewip.js</em>, and the IP should be added to all 7 profiles.</li>
                </ol>
            </li>
            <li>Finally run the <em>testAutomater.js</em> file using <em>node testAutomater.js</em>, and the experiment
                should start for the duration specified per row in the <em>Examples.xlsx</em> file.</li>
        </ul>
    </li>
</ol>
