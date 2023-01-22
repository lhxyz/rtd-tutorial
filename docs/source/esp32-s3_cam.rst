esp32-s3_cam
============

.. note::

   This project is under active development.



.. code-block:: console
    
 def do_connect():
    import network
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    if not wlan.isconnected():
        print('connecting to network...')
        wlan.connect('++', '++')
        while not wlan.isconnected():
            pass
    print('network config:', wlan.ifconfig())

 do_connect()

 import webrepl
 webrepl.start(port=8255,password='++')

  
 import blynklib
 blynk = blynklib.Blynk('++', server='++', port=8080)
 @blynk.handle_event("connect")
 def connect_handler():
    print('Blynk connected')

 @blynk.handle_event("disconnect")
 def connect_handler():
    print('Blynk disconnected')
    

 @blynk.handle_event('write V1')
 def write_virtual_pin_handler(pin, value_1):
    if value_1 == ['1']:
        print('pwm0.duty', value_1)
    else:
        print('pwm0.duty', value_1)
        
 CMD_LIST = ['logo', 'version', 'sysinfo', 'ls']


 @blynk.handle_event('write V2')
 def write_handler(pin, values):
    if values:
        in_args = values[0].split(' ')
        cmd = in_args[0]
        cmd_args = in_args[1:]

        if cmd == 'help':
            output = ' '.join(CMD_LIST)
        elif cmd == CMD_LIST[0]:
            output = blynklib.LOGO
        elif cmd == CMD_LIST[1]:
            output = blynklib.__version__
        elif cmd == CMD_LIST[2]:
            output = uos.uname()
        elif cmd == CMD_LIST[3]:
            arg = cmd_args[0] if cmd_args else ''
            output = uos.listdir(arg)
        else:
            output = "[ERR]: Not supported command '{}'".format(values[0])

        blynk.virtual_write(pin, output)
        blynk.virtual_write(pin, '\n')


 while True:
    blynk.run()

