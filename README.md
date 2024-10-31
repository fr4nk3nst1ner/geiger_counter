# Geiger Counter for Raspberry Pi 

- This is heavily based off [this](https://github.com/chrisys/background-radiation-monitor) script

## Instructions 

- Purchase your geiger counter (I used [this](https://www.amazon.com/dp/B09X1H5CW4?ref=ppx_yo2ov_dt_b_fed_asin_title) one)
- Setup your Raspberry pi so you can ssh in (I used an old Raspberry Pi3)
- Using a pinout diagram (e.g., [this](https://hackster.imgix.net/uploads/attachments/218603/6sQiFTKXhZptFiGnPlsc.png)), hook up the counter to an availbe GPIO pin
    - The script I provide uses GPIO 23 (pin 16)
    - Up to you if you want to power the counter using 5v+gnd from the pi or use the external power source 

### Library Requirements

- **Python 3** and **virtual environment** support installed on your Raspberry Pi.
- The following Python packages installed in your virtual environment (`requirements.txt`):
  - `influxdb`
  - `spidev`
  - `RPi.GPIO`
  - `rpi-lgpio`
  
- **Paths and Files**
  - As an example, assumption is `counter.py` and `exixe.py` are located in `/home/fr4nk3nst1ner/geiger/`.
  - My example virtual environment directory is located at `/home/fr4nk3nst1ner/geiger/venv/`.

---

### Step 1: Set Up the Virtual Environment

1. **Create and activate the Virtual Environment and install deps**:

   ```
   git clone https://github.com/fr4nk3nst1ner/geiger_counter.git /home/fr4nk3nst1ner/geiger
   cd /home/fr4nk3nst1ner/geiger
   python3 -m venv venv
   source venv/bin/activate
   pip install influxdb spidev RPi.GPIO rpi-lgpio
   ```

---

### Step 2: Create a Systemd Service File

1. **Open a New Service File**:

   ```
   sudo vim /etc/systemd/system/geiger_counter.service
   ```

2. **Add the Following Configuration to the Service File**:

   Replace paths as necessary if your setup differs.

   ```
   [Unit]
   Description=Geiger Counter Service
   After=network.target

   [Service]
   Type=simple
   User=fr4nk3nst1ner
   WorkingDirectory=/home/fr4nk3nst1ner/geiger
   ExecStart=/home/fr4nk3nst1ner/geiger/venv/bin/python /home/fr4nk3nst1ner/geiger/counter.py
   Restart=on-failure
   Environment=PYTHONPATH=/home/fr4nk3nst1ner/geiger 

   [Install]
   WantedBy=multi-user.target
   ```

---

### Step 3: Enable and Start the Service

1. **Reload systemd, enable, and start the service**:

   ```
   sudo systemctl daemon-reload
   sudo systemctl enable geiger_counter.service
   sudo systemctl start geiger_counter.service
   ```

---

### Step 4: Check the Service Status and Logs

1. **Check the Service Status and view logs**:

   ```
   sudo systemctl status geiger_counter.service
   sudo journalctl -u geiger_counter.service -f
   ```

---

## Troubleshooting

- First thing, check that gpio output is registering changes to voltage on specified gpio pin 
- If this holds steady `1` or `0`, then the wiring and connection of your Geiger counter module are incorrect.
    - `1` and `0` represent HIGH (3.3V or 5V) and LOW (GND), respectively, so if it's not switching between these values, there might be an issue with how you have connected the wires to the GPIO pins on your Raspberry Pi.

    ```
    watch -n 0.1 gpio -g read 23
    ```

- **Service Fails to Start**: Check the logs for detailed error messages:

  ```
  sudo journalctl -u geiger_counter.service -xe
  ```

- **Verify Paths**: Ensure that the paths to `counter.py`, `exixe.py`, and the virtual environment are correct in both your systemd service file and any import statements.

- **Confirm Dependencies**: If you encounter `ModuleNotFoundError` errors, activate the virtual environment manually, install any missing packages, and try running `counter.py` directly to confirm it works.

---

This guide should help you set up and manage the `counter.py` script as a systemd service on boot. Let me know if you encounter any issues or need further assistance! 
