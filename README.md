
<!---

This README is automatically generated from the comments in these files:
platinum-bluetooth-characteristic.html  platinum-bluetooth-device.html  platinum-bluetooth-elements.html

Edit those files, and our readme bot will duplicate them over here!
Edit this file, and the bot will squash your changes :)

-->

_[Demo and API Docs](https://elements.polymer-project.org/elements/platinum-bluetooth)_


##&lt;platinum-bluetooth-characteristic&gt;


The `<platinum-bluetooth-characteristic>` element allows you to [read
and write characteristics on nearby bluetooth devices][1] thanks to the
young [Web Bluetooth API][2]. It is currently only partially implemented
in Chrome OS 45 behind the experimental flag
`chrome://flags/#enable-web-bluetooth`.

`<platinum-bluetooth-characteristic>` needs to be a child of a
`<platinum-bluetooth-device>` element.

For instance, here's how to read battery level from a nearby bluetooth
device advertising Battery service:

```html
<platinum-bluetooth-device services-filter='["battery_service"]'>
  <platinum-bluetooth-characteristic
      service='battery_service'
      characteristic='battery_level'>
  </platinum-bluetooth-characteristic>
</platinum-bluetooth-device>
```
```js
var bluetoothDevice = document.querySelector('platinum-bluetooth-device');
var batteryLevel = document.querySelector('platinum-bluetooth-characteristic');

button.addEventListener('click', function() {
  bluetoothDevice.request().then(function() {
    return batteryLevel.read().then(function(value) {
      var data = new DataView(value);
      console.log('Battery Level is ' + data.getUint8(0) + '%');
    });
  })
  .catch(function(error) { });
});
```

Here's another example on how to reset energy expended on nearby
bluetooth device advertising Heart Rate service:

```html
<platinum-bluetooth-device services-filter='["heart_rate"]'>
  <platinum-bluetooth-characteristic
      service='heart_rate'
      characteristic='heart_rate_control_point'>
  </platinum-bluetooth-characteristic>
</platinum-bluetooth-device>
```
```js
var bluetoothDevice = document.querySelector('platinum-bluetooth-device');
var heartRateCtrlPoint = document.querySelector('platinum-bluetooth-characteristic');

button.addEventListener('click', function() {
  bluetoothDevice.request().then(function() {
    // Writing 1 is the signal to reset energy expended.
    var resetEnergyExpended = new Uint8Array([1]);
    return heartRateCtrlPoint.write(resetEnergyExpended).then(function() {
      console.log('Energy expended has been reset');
    });
  })
  .catch(function(error) { });
});
```

It is also possible for `<platinum-bluetooth-characteristic>` to fill in
a data-bound field in response to a read.

```html
<platinum-bluetooth-device services-filter='["heart_rate"]'>
  <platinum-bluetooth-characteristic
      service='heart_rate'
      characteristic='body_sensor_location'
      value={{bodySensorLocation}}>
  </platinum-bluetooth-characteristic>
</platinum-bluetooth-device>
...
<span>{{bodySensorLocation}}</span>
```
```js
var bluetoothDevice = document.querySelector('platinum-bluetooth-device');
var bodySensorLocation = document.querySelector('platinum-bluetooth-characteristic');

button.addEventListener('click', function() {
  bluetoothDevice.request().then(function() {
    return bodySensorLocation.read()
  })
  .catch(function(error) { });
});
```

You can also use changes in `value` to drive characteristic writes when
`auto-write` property is set to true.

```html
<platinum-bluetooth-device services-filter='["heart_rate"]'>
  <platinum-bluetooth-characteristic
      service='heart_rate'
      characteristic='heart_rate_control_point'
      auto-write>
  </platinum-bluetooth-characteristic>
</platinum-bluetooth-device>
```
```js
var bluetoothDevice = document.querySelector('platinum-bluetooth-device');
var heartRateCtrlPoint = document.querySelector('platinum-bluetooth-characteristic');

button.addEventListener('click', function() {
  bluetoothDevice.request().then(function() {
    // Writing 1 is the signal to reset energy expended.
    heartRateCtrlPoint.value = new Uint8Array([1]);
  })
  .catch(function(error) { });
});

heartRateCtrlPoint.addEventListener('platinum-bluetooth-auto-write-error',
    function(event) {
  // Handle error...
});
```

[1]: https://developers.google.com/web/updates/2015/07/interact-with-ble-devices-on-the-web
[2]: https://github.com/WebBluetoothCG/web-bluetooth



##&lt;platinum-bluetooth-device&gt;


The `<platinum-bluetooth-device>` element allows you to [discover nearby
bluetooth devices][1] thanks to the young [Web Bluetooth API][2]. It is
currently only partially implemented in Chrome OS 45 behind the
experimental flag `chrome://flags/#enable-web-bluetooth`.

`<platinum-bluetooth-device>` is used as a parent element for
`<platinum-bluetooth-characteristic>` child elements.

For instance, here's how to request a nearby bluetooth device advertising
Battery service :

```html
<platinum-bluetooth-device
    services-filter='["battery_service"]'>
</platinum-bluetooth-device>
```
```js
button.addEventListener('click', function() {
  document.querySelector('platinum-bluetooth-device').request()
  .then(function(device) { console.log(device.name); })
  .catch(function(error) { console.error(error); });
});
```

[1]: https://developers.google.com/web/updates/2015/07/interact-with-ble-devices-on-the-web
[2]: https://github.com/WebBluetoothCG/web-bluetooth


