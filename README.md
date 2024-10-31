# Geiger Counter for Raspberry Pi 

- This is heavily based off [this](https://github.com/chrisys/background-radiation-monitor) script

## Instructions 

- Purchase your geiger counter (I used [this](https://www.amazon.com/dp/B09X1H5CW4?ref=ppx_yo2ov_dt_b_fed_asin_title) one)
- Setup your raspberry pi and ssh in 
- Clone the repository 

```
git clone https://github.com/fr4nk3nst1ner/geiger_counter.git
```

- Using a pinout diagram (e.g., [this](https://hackster.imgix.net/uploads/attachments/218603/6sQiFTKXhZptFiGnPlsc.png)), hook up the counter to an availbe GPIO pin
- The script i provide uses GPIO 23 (pin 16)
- Install the requirements 
