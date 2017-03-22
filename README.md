# Bluetooth2


Удалось добиться результата. Изменил способ отправки команд на построчно, а одной строкой. Для этого добавил еще один разделитель, который позволяет вначале разбить на отдельные команды, а потом команду на значение и название параметра.

Код ардуино:
Java
Выделить код

1
2
3
4
5
6
7
8
9
10
11
12

	

void loop() {
  for (int i=0; i<100; i++){
    Serial.print("temp=");
    Serial.print(i);
    Serial.print("|");
    Serial.print("time=");
    Serial.print(i);
    Serial.print("|");
    Serial.println();
    delay(500);
    }
}

Код приложения:
Java
Выделить код

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30

	

h = new Handler() {
            public void handleMessage(android.os.Message msg) {
                switch (msg.what) {
                    case RECIEVE_MESSAGE:                                                   // если приняли сообщение в Handler
                        byte[] readBuf = (byte[]) msg.obj;
                        String strIncom = new String(readBuf, 0, msg.arg1);
                        sb.append(strIncom);                                                // формируем строку
                        int endOfLineIndex = sb.indexOf("\r\n");                            // определяем символы конца строки
                        if (endOfLineIndex > 0) {                                            // если встречаем конец строки,
                            String sbprint = sb.substring(0, endOfLineIndex);               // то извлекаем строку
                            sb.delete(0, sb.length());                                      // и очищаем sb
                            Log.d(TAG, sbprint);
                            String[] result = sbprint.split("\\|");
                            for (int i=0; i<result.length; i++){
                                String[] command = result[i].split("=");
                            if(command[0].compareTo("temp")==0) {
                                txtArduino.setText("Temp: " + command[1]);             // обновляем TextView
                            }
                            if(command[0].compareTo("time")==0) {
                                time.setText("time: " + command[1]);             // обновляем TextView
                            }
                            }
                            btnOff.setEnabled(true);
                            btnOn.setEnabled(true);
                        }
                        //Log.d(TAG, "...Строка:"+ sb.toString() +  "Байт:" + msg.arg1 + "...");
                        break;
                }
            };
        };
