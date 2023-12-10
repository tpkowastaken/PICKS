# How to make a custom keyboard layout in KDE
- All you have to look at are these files:
  - ```/usr/share/X11/xkb/symbols/```
  - ```/usr/share/X11/xkb/rules/evdev.xml```
  - ```/usr/share/X11/xkb/rules/evdev.lst```

1. First in ```/usr/share/X11/xkb/symbols``` you can find a lot of keyboard layouts categorized by the language. There are a lot of gems in these files take a look if you don't like a layout already if you do skip to 3.
2. In case you haven't decided for a keyboard layout there We will create a new block for our layout.
  - choose the language you want to have your keyboard layout based on
  - open that file and create a new block with an ID and a name:
  ```c
xkb_symbols "keyboard_id" {// unique keyboard id
    include "us" // the keyboard you are based on
    name[Group1]= "my keyboard"; // the keyboard name that will be shown in the menus

    // your configuration will later go here

    include "level3(ralt_switch)" // optional switch to allow alt+gr and alt+gr+shift to be used
};
```
3. We will come back here later. First we have to make it readable by the system. open
```/usr/share/X11/xkb/rules/evdev.lst```
- Search for the file name of the file where you put the keyboard layout block in (in my case it's cz) and put a new entry in there like this:
```keyboard_id     cz: my keyboard```
- while replacing the `keyboard_id` `cz` and `my keyboard` with your values
- ![image](https://github.com/tpkowastaken/PICKS/assets/85837981/a21fbe7f-1a7a-4bef-b239-39ad8d0dbc9d)

4. Do a similar task in 
```/usr/share/X11/xkb/rules/evdev.xml```
![image](https://github.com/tpkowastaken/PICKS/assets/85837981/52b92336-de43-47b2-934b-0adbc25a2881)

5. If you have done everything correctly it should now appear in the keyboard layouts:
![image](https://github.com/tpkowastaken/PICKS/assets/85837981/b95ee71c-16ab-4bf5-8ba1-ffa3afc8c864)

6. apply this keyboard layout. You can now type with the layout! But you propably want to make changes to the base layout (in our case us)

7. Now we go back to 2. - you will need to define any keys that will be different from the layout you are basing your layout of (in the case here it is the us keyboard)
```c
xkb_symbols "keyboard_id" {// unique keyboard id
    include "us" // the keyboard you are based on
    name[Group1]= "my keyboard"; // the keyboard name that will be shown in the menus

    key <AE01>	{[      plus,     exclam,            1,        1 ]};
    key <AE02>	{[    ecaron,         at,            2,   Ecaron ]};
    key <AE03>	{[    scaron, numbersign,            3,   Scaron ]};
    key <AE04>	{[    ccaron,     dollar,            4,   Ccaron ]};
    key <AE05>	{[    rcaron,    percent,            5,   Rcaron ]};
    key <AE06>	{[    zcaron,asciicircum,            6,   Zcaron ]};
    key <AE07>	{[    yacute,  ampersand,            7,   Yacute ]};
    key <AE08>	{[    aacute,   asterisk,            8,   Aacute ]};
    key <AE09>	{[    iacute,  parenleft,            9,   Iacute ]};
    key <AE10>	{[    eacute, parenright,            0,   Eacute ]};
    key <AE11>	{[     minus, underscore,     NoSymbol, NoSymbol ]};
    key <AE12>	{[     equal,       plus,   dead_caron, dead_acute]};

    include "level3(ralt_switch)" // optional switch to allow alt+gr and alt+gr+shift to be used
};
```

## Explanation of what is going on
- ```<AE01>``` - the identifier of the physical key. refer to other keyboard layouts in the files for reference
- ```{[ first_key, second_key, third_key, fourth_key]);```
  - first_key - key when pressed without other buttons presses
  - second_key - key when pressed with shift
  - third_key - key when pressed with alt+gr
  - fourth_key - key when pressed with alt+gr+shift
- now if you have ommited ```include "level3(ralt_switch)"``` third and fourth key won't do anything and therefore you should have just two keys like so:
    ```key <AE10>	{[    eacute, parenright]};```
- reorder the layouts and apply to see the changes after saving the file (or somehow other reload the keyboard layouts in your system. In the worst case just reboot)
![image](https://github.com/tpkowastaken/PICKS/assets/85837981/a620a0d1-c353-4d8d-8ab4-96657e20862c)


## Example of my keyboard layout:
- ```/usr/share/X11/xkb/symbols/cz```
```c
partial alphanumeric_keys
xkb_symbols "cz_prog" {

    // my custom layout
    // *****************************************************
    // * This keyboard layout provides comfortable typing  *
    // * For English, Czech and German while also being    *
    // * us-symbol based which ensures nice experience     *
    // * when programming or doing other tasks             *
    // * It's based on US because the majority of people   *
    // * Use the US keyboard layout                        *
    // *****************************************************



    include "us(basic)"
    name[Group1]= "Czech (QWERTY, czech programming)";



    key <AE01>	{[      plus,     exclam,            1,        1 ]};
    key <AE02>	{[    ecaron,         at,            2,   Ecaron ]};
    key <AE03>	{[    scaron, numbersign,            3,   Scaron ]};
    key <AE04>	{[    ccaron,     dollar,            4,   Ccaron ]};
    key <AE05>	{[    rcaron,    percent,            5,   Rcaron ]};
    key <AE06>	{[    zcaron,asciicircum,            6,   Zcaron ]};
    key <AE07>	{[    yacute,  ampersand,            7,   Yacute ]};
    key <AE08>	{[    aacute,   asterisk,            8,   Aacute ]};
    key <AE09>	{[    iacute,  parenleft,            9,   Iacute ]};
    key <AE10>	{[    eacute, parenright,            0,   Eacute ]};
    key <AE11>	{[     minus, underscore,     NoSymbol, NoSymbol ]};
    key <AE12>	{[     equal,       plus,   dead_caron, dead_acute]};


    key <AD03>	{[         e,          E,     EuroSign, EuroSign ]};

    key <AD05>	{[         t,          T,       tcaron,   Tcaron ]};

    key <AD07>	{[         u,          U,        uring,    Uring ]};
    key <AD08>	{[         i,          I,          252,      220 ]};
    key <AD09>	{[         o,          O,       oacute,   Oacute ]};
    key <AD10>	{[         p,          P,          246,      214 ]};
    key <AD11>	{[bracketleft, braceleft,       uacute,   Uacute ]};

    key <BKSL>  {[ backslash,        bar,    ampersand, asterisk ]};


    key <AC01>	{[         a,          A,          196,      228 ]};
    key <AC02>	{[         s,          S,       ssharp,   ssharp ]};
    key <AC03>	{[         d,          D,       dcaron,   Dcaron ]};
    key <AC10>	{[ semicolon,      colon,        grave,    grave ]};
    key <AC11>	{[ apostrophe,  quotedbl,      section, section  ]};

    key <AB06>	{[         n,          N,       ncaron,   Ncaron ]};


    key <AB02>	{[         x,          X,   numbersign, numbersign]};
    key <AB03>	{[         c,          C,    ampersand, ampersand]};
    key <AB04>	{[         v,          V,           at,       at ]};
    key <AB08>	{[     comma,       less,          215,      215 ]};
    key <AB09>	{[    period,    greater,          247,      247 ]};
    key <AB10>	{[     slash,   question,     NoSymbol, NoSymbol ]};
    include "level3(ralt_switch)"
};
```
- ```/usr/share/X11/xkb/rules/evdev.xml```

![image](https://github.com/tpkowastaken/PICKS/assets/85837981/97d28ac3-a728-474c-8f78-fcb289ab5de7)

- ```/usr/share/X11/xkb/rules/evdev.lst```

![image](https://github.com/tpkowastaken/PICKS/assets/85837981/90cba28b-7b4e-4bf5-99c5-3e3bf7ef6a21)

