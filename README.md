# EVGUI

## system config
  * Operating System	: Ubuntu 16.04
	* arm-none-eabi-gcc	: (version) 4.9.3 (20150529)
  * gcc								: (version) 5.4.0 (20160609)
  * make							: (version) GNU Make 4.1

## install
### git repo
  > $ git clone https://github.com/Joouis/EVGUI.git # proj
  > $ git clone https://github.com/Joouis/EVGUI_Viewer.git # viewer
### GNU toolchain
  > $ sudo apt-get install gcc-arm-none-eabi # ARM embedded gcc compiler
### enviornment setting
  > $ sudo apt-get install libx11-dev # x window library
  > $ sudo apt-get install openocd # flash tool
	* tools [optional]
		+ screen, tmux 	: serial communication
		+ gdb, cgdb			: gcc debugger (via `make debug`)
### build
  * proj
    > $ cd proj/
    > $ make
    > $ make flash # flash binary files into MCU (Micro Control Unit)
  * viewer
    > $ cd viewer/
    > $ make
    > $ ./xdemo # show demo svg on linux

## process
### work flow
	----------------------			-------------------			----------------			-----------
	| presentation layer |	=>	|	rendering layer |	=> 	|	output layer |	=>	| drivers |
	----------------------			-------------------			----------------			-----------
	* presentation layer : libsvgtiny, ezxml
	* render layer			 : twin
	* output layer			 : uGUI
### svg render
	* main()
		+	twin_fbdev_create()							: initialize hardware
		+ twin_svg_index_start()					: svg parsing and drawing
			-	svgtiny_parse()								: parse svg to drawing information
			-	render_path() / render_text()	: call vector graphics library to finish drawing
		+ twin_fbdev_activate()						: first paint/update screen of hardware
		+ twin_dispatch()									: global loop for updating (window, screen, process input)

## usage
	* manually copy svg (markup code) codes into apps/twin_svg.c via 'vim'
	* parsing svg codes into drawing information for vector graphics library
	* flash binary files into MCU (Micro Control Unit)

## files
  * proj/
    + apps/ 											- user code to build GUI
		+ backend/										- drivers for hardware (stm32f429)
		+ cmsis/											- general protocol to use the core of MCU
		+ cmsis_boot/									- boot config
		+ gdb.script									- debug config
		+ ImgToString.sh							- convert svg codes to string
		+ ld/													- linking scripts (ref: https://www.math.utah.edu/docs/info/ld_3.html)
		+ libsvgtiny/									- svg and xml parser (analyze svg files for vector engine)
		+ STM32F4xx_StdPeriph_Driver/	- STM32F4xx Standard peripheral drivers
		+ syscalls/										- a malloc function for using external sdram
		+ twin/												- vector graphics library
		+ uGUI/												- mini GUI library as driver interface (ref: http://www.embeddedlightning.com/ugui/)

  * viewer/
    + apps/ 											- user code to build GUI
		+ backend/										- drivers for hardware (x11)
		+ ImgToString.sh							- convert svg codes to string
		+ libsvgtiny/									- svg and xml parser (analyze svg files for vector engine)
		+ src/												- vector graphics library (twin)
		+ include/										- some extra twin libraries

## ref
### project (dropbox paper)
  * https://paper.dropbox.com/doc/Embedded-Graphics-Entry-cVe71Nm0Kk8OLJlN6Gst2
### stm32f4 (datasheet)
  * http://www.st.com/content/ccc/resource/technical/document/datasheet/03/b4/b2/36/4c/72/49/29/DM00071990.pdf/files/DM00071990.pdf/jcr:content/translations/en.DM00071990.pdf
### stm32f4 (reference manual)
  * http://www.st.com/content/ccc/resource/technical/document/reference_manual/3d/6d/5a/66/b4/99/40/d4/DM00031020.pdf/files/DM00031020.pdf/jcr:content/translations/en.DM00031020.pdf
