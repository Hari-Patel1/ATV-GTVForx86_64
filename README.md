

  <h1>ATV-GTG-for-x86_64</h1>
  <p><strong>Android TV and Google TV ported to x86_64 devices (PCs, laptops, etc.)</strong></p>

  <p>This project brings Android and Google TV to x86 and x64 platforms, allowing you to run TV-optimized Android OS on standard laptops, desktops, and other non-ARM devices.</p>

  <hr>

  <h2>🔗 Download</h2>
  <p>All necessary files can be found here: <strong><a href="#">https://terabox.com/s/1KLFes4RqTjn8IE3nu2VZUA</a></strong><br>
  <em>Navigate to the <code>ATV-GTG-for-x86_64</code> folder within the link.</em></p>

  <hr>

  <h2>💿 Setup Instructions</h2>

  <h3>1. Flash the Image</h3>
  <ul>
    <li>Use <strong>Rufus</strong> to flash the provided image onto a USB drive or external hard disk.</li>
    <li>Boot the target system using this drive.</li>
    <li>⚠️ <strong>Important:</strong> You must keep the flashed device attached at all times while using the OS.</li>
  </ul>

  <h3>2. Persistent Storage Fix (Saving User Data)</h3>
  <p>By default, the system doesn't retain any changes after reboot. To enable persistent storage:</p>
  <ol>
    <li>Download the appropriate <code>data.zip</code> file (choose the correct size for your use).</li>
    <li>Unzip it and copy the contents into the <strong>system partition</strong> of the flashed drive.</li>
  </ol>

  <h4>⚠️ File Size Limit Warning</h4>
  <p>The system partition usually doesn't accept files larger than <strong>4GB</strong>. To work around this:</p>
  <ul>
    <li>Split large files into smaller parts using an archive format (e.g., <code>.zip</code>, <code>.7z</code>, etc.).</li>
    <li>Copy the parts individually to the partition.</li>
    <li>Unzip them <strong>inside the device</strong> after boot.</li>
  </ul>

  <hr>

  <h2>🔊 HDMI Audio Output Setup</h2>
  <p>To enable HDMI audio output on your device, follow the steps below.</p>

  <h3>📁 Step 1: Copy Required Files</h3>
  <ul>
    <li>In the TeraBox directory, go to: <code>ATV-GTG-for-x86_64 &gt; audio</code></li>
    <li>Copy the entire <strong>audio</strong> folder onto another storage device (e.g., USB stick).</li>
  </ul>

  <h3>🛠 Step 2: Analyze HDMI Output</h3>
  <ol>
    <li>Boot into your Android TV device.</li>
    <li>Install the following tools:
      <ul>
        <li><strong>FX File Manager</strong> (grant it root access).</li>
        <li><strong>SmartPack Kernel Manager</strong> (also grant root access).</li>
      </ul>
    </li>
    <li>Install the APK file found in the <code>audio</code> folder.</li>
    <li>Open the APK and navigate to the <strong>Output</strong> or <strong>Sound</strong> tab.
      <ul>
        <li>Identify your <strong>HDMI output rail number</strong> (e.g., <code>0</code>, <code>1</code>, <code>2</code>).</li>
        <li>Make a note of this number — you'll need it in the next steps.</li>
      </ul>
    </li>
  </ol>

  <h3>🔍 Step 3: Run the Detection Script</h3>
  <ol>
    <li>Copy the <code>test.wav</code> and the correct detection script from the <code>audio</code> folder to your device’s internal filesystem.</li>
    <li>Using <strong>FX File Manager</strong>, run the script with <strong>root permissions</strong> (tap-and-hold → Execute with root).</li>
    <li>The script will loop through multiple <strong>PCM outputs</strong>, attempting to play the test sound.</li>
    <li>When audio successfully plays from your HDMI device:
      <ul>
        <li>Note the <strong>PCM number</strong> that worked.</li>
        <li>Also confirm the <strong>HDMI rail</strong> you used.</li>
      </ul>
    </li>
  </ol>

  <h3>✏️ Step 4: Modify the Script for Your Output</h3>
  <p>Locate this line in the script:</p>
  <pre><code>setprop hal.audio.out.hdmi pcmC0D7p</code></pre>

  <p>Update it with the correct HDMI rail and PCM number:</p>
  <ul>
    <li>The number after <code>C</code> is your HDMI rail.</li>
    <li>The number after <code>D</code> is the working PCM.</li>
  </ul>

  <h4>Example</h4>
  <p>If HDMI rail <code>2</code> and PCM <code>5</code> worked, modify the line to:</p>
  <pre><code>setprop hal.audio.out.hdmi pcmC2D5p</code></pre>

  <h3>⚡ Optional: Speed Up the Script</h3>
  <p>Once you've identified the correct PCM, you can optimize the detection script to test only that value.</p>

  <h4>Original Loop</h4>
  <pre><code>for pcm in 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20; do
    echo "🔊 Testando PCM $pcm..." | tee -a "$LOG"

    if aplay -D plughw:0,$pcm "$AUDIO"; then
        echo "✅ PCM $pcm OK" | tee -a "$LOG"
    else
        echo "❌ PCM $pcm FALHOU" | tee -a "$LOG"
    fi

    echo "------------------------------" >> "$LOG"
    sleep 2
done</code></pre>

  <h4>Optimized Loop (for PCM 5)</h4>
  <pre><code>for pcm in 5; do
    echo "🔊 Testando PCM $pcm..." | tee -a "$LOG"

    if aplay -D plughw:0,$pcm "$AUDIO"; then
        echo "✅ PCM $pcm OK" | tee -a "$LOG"
    else
        echo "❌ PCM $pcm FALHOU" | tee -a "$LOG"
    fi

    echo "------------------------------" >> "$LOG"
    sleep 2
done</code></pre>

  <hr>

  <h2>✅ Final Step</h2>
  <p>You're done! 🎉</p>
  <p>Your <strong>Android or Google TV system</strong> is now fully functional on your x86 or x64 device, with:</p>
  <ul>
    <li>Persistent storage support</li>
    <li>HDMI audio output enabled</li>
    <li>Full Google TV or Android TV experience, including the <strong>Google Play Store</strong></li>
  </ul>

  <hr>

  <h2>📌 Notes</h2>
  <ul>
    <li>This project is <strong>under active development</strong>.</li>
    <li>Feel free to open issues or contribute via pull requests.</li>
    <li>Feedback is always welcome!</li>
  </ul>

  <hr>

  <h2>📜 License</h2>
  <p>This project is released under the <a href="#">MIT License</a>.</p>

  <hr>

  <h2>🤝 Contributing</h2>
  <p>Want to help improve ATV-GTG-for-x86_64?</p>
  <ol>
    <li>Fork this repo</li>
    <li>Create a new branch (<code>git checkout -b feature-name</code>)</li>
    <li>Commit your changes (<code>git commit -m "Add feature"</code>)</li>
    <li>Push to your branch (<code>git push origin feature-name</code>)</li>
    <li>Open a pull request!</li>
  </ol>

  <hr>

  <h2>📷 Screenshots (Optional)</h2>
  <blockquote>
    <p><em>Coming soon. You can help by submitting your own screenshots!</em></p>
  </blockquote>

  <hr>

  <h2>👥 Credits</h2>
  <ul>
    <li><strong>Project Lead:</strong> Hari Patel</li>
    <li><strong>Porting & Development:</strong> Hari Patel</li>
    <li><strong>Documentation & Setup Guide:</strong> Hari Patel</li>
    <li><strong>Special thanks:</strong> The open-source Android-x86 and TV development communities</li>
  </ul>

  <p>Thanks for using <strong>ATV-GTG-for-x86_64</strong>!<br>
  Happy streaming! 📺</p>

</body>
</html>
