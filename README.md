# oxonBtn-decoder-ttn-v3
```
function decodeUplink(input) {
  var  data = {};
  data.rawPL = input.bytes;

  switch (input.bytes[0]) {

     case 0x30:
       buttonboardStateUpdate(input.bytes,data);
      break;

     default:
       buttonboardStateUpdate(input.bytes,data);
      break;
  }
return {
    data: data 
	};
}
/* ID 0x30 */
function buttonboardStateUpdate(bytes,data) {
  var button = "0x" + ((bytes[1] < 16 ? "0" : "") + bytes[1].toString(16)).toUpperCase();
  var hbIRQ = !!bytes[2];
  var accIRQ = !!bytes[3];
  var appMode = bytes[4];
  var enBtns = "0x" + ((bytes[5] < 16 ? "0" : "") + bytes[5].toString(16)).toUpperCase();
  var bat = bytes[6];
  var temp = bytes[7];
  var accX = bytes[8] * 256 + bytes[9];
  var accY = bytes[10] * 256 + bytes[11];
  var accZ = bytes[12] * 256 + bytes[13];
  accX = accX < 32767 ? (2 / 8191) * accX : (-2 / 8192) * (65536 - accX);
  accY = accY < 32767 ? (2 / 8191) * accY : (-2 / 8192) * (65536 - accY);
  accZ = accZ < 32767 ? (2 / 8191) * accZ : (-2 / 8192) * (65536 - accZ);
  accX = Math.round((accX + 2.7755575615628914e-17) * 1000) / 1000;
  accY = Math.round((accY + 2.7755575615628914e-17) * 1000) / 1000;
  accZ = Math.round((accZ + 2.7755575615628914e-17) * 1000) / 1000;
  if (bytes.length > 14) var prodFam = (bytes[14] < 16 ? "0" : "") + bytes[14].toString(16);
  if (bytes.length > 15) var prodType = (bytes[15] < 16 ? "0" : "") + bytes[15].toString(16);
  if (bytes.length > 16) var prodVar = (bytes[16] < 16 ? "0" : "") + bytes[16].toString(16);
  //var prodId = "0x" + (prodFam + prodType + prodVar).toUpperCase();
  if (bytes.length > 18) var hwVers = "V" + bytes[17].toString(16) + "." + (bytes[18]/16).toString(16) + "." + (bytes[18]%16).toString(16);
  if (bytes.length > 20) var fwVers = "V" + bytes[19].toString(16) + "." + (bytes[20]/16).toString(16) + "." + (bytes[20]%16).toString(16);
 
  var decoded = {
    button: button,
    hbIRQ: hbIRQ,
    accIRQ: accIRQ,
    appMode: appMode,
    enBtns: enBtns,
    bat: bat,
    temp: temp,
    accX: accX,
    accY: accY,
    accZ: accZ,
    //prodId: prodId,
    hwVers: hwVers,
    fwVers: fwVers,
  };
  Object.assign(data,decoded);
}
```
