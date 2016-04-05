Originally written and coded by Sean Mullan

Modified by Michael Vue
Fall 2015

Modified by Jon Bisila
Winter/Spring 2016

WebToPDF
CURRENT ISSUES:
1) "Issue found on page:wkhtmltopdf reported an error:
Mon Mar 28 20:51:52 unreg-30-152.dyn.carleton.edu wkhtmltopdf[3817] <Error>: CGContextSetShouldAntialias: invalid context 0x0" 

2) Issues with creating a folder for the discussion topic. See Sept 30 of History course of 2012-13 moodle page

3) Issues with redirect links that we cannot differentiate between external links and "the link" module on moodle. We still want to scrape the moodle page. Plan: use python "requests" module to check if the link redirects to an external page and if it does, just print the link on the page and don't go to the page and if it stays within moodle, proceed to that page and scrape it. http://stackoverflow.com/questions/20475552/python-requests-library-redirect-new-url

For external links, we probably want to change the "instance name" to the external link, since there isn't a better way to represent that link/print it. 


Contents:
	The WebToPDF folder should contain these items in order to function: 
		WebToPDF.py
		README.txt
		bs4
		wkhtmltox-0.12.1_osx-carbon-i386.pkg
		[NOTE: wkhtml version from 6-27-2014, updates available at http://			wkhtmltopdf.org/downloads.html]

Set-Up Instructions (For Mac OSX):
	[NOTE: Python should come installed on OSX]
	[NOTE: These instructions only need to be followed once on a machine.]
	Install wkhtml using the .pkg included in the WebToPDF file.
	Open Terminal
	Type in these instructions one line at a time:
		sudo easy_install pip
		export CFLAGS=-Qunused-arguments
		export CPPFLAGS=-Qunused-arguments
		sudo -E pip install lxml
		[IF THIS FAILS: type in: export ARCHFLAGS='-arch i386 -arch x86_64' and try again]
		sudo pip install pdfkit
	[NOTE: These commands install pip (an installer), lxml, and pdfkit (python modules)]
	[NOTE: you need to be on the admin account and have the password to use the sudo commands]
	Go to Finder and press Shift+Command+G
	Enter /Library/Python/#.#/site-packages/pdfkit into the dialogue box where #.# is your current python version
	[NOTE: This can by found by typing ‘python’ into the terminal window. It is in the form Python #.#.#,
	 on the first line of output, and you only need the first two numbers.]
	Open pdfkit.py in a text editor
	Replace line 103 with input = self.source.source.read() 
	[NOTE: line 103 is previously: input = self.source.source.read().encode('utf-8’)]
	[NOTE: If you could not find the correct version of python, try looking in 
		/Library/Frameworks/Python.framework/Versions/#.#/lib/python#.#/site-packages/pdfkit
		where #.# is your python version]

How To Use (Single Course):
	Go to Terminal and type in “cd “, drag the WebToPDF folder into the Terminal window and hit enter.
	[NOTE: The space after cd is important]
	Type in “python WebToPDF.py” and hit enter
	Type in your username and then password when prompted.
	When prompted, type in the short form of the class to scrape.
	[NOTE: That is the format class-section-term ie. cs257-00-s14 for spring 2014 software design]
	Do not delete the file “temp” that appears while the program is running. It will remove itself.

How To Use (Multiple Courses):
	Make a new .txt file called “courses.txt” in the WebToPDF folder with all of the courses’ short names that
	you want to save on separate lines with no punctuation.
	Follow the above steps for a single course.
	[NOTE:If the program still asks for the short name of the class, make sure that the file is saved in the
	right location and is saved as “courses.txt”]
	[NOTE:The program runs through one class at a time, so the percentages reflect only the current class.]
	
Output:
	The files will be stored into a subdirectory of the WebToPDF folder.
	The directory the files are stored in will use the short form of the class as its title.
	[NOTE: If the same class is printed multiple times, later versions will get a number after to identify them.]
	When looking at the output, use either the “Date Added” or “Date Modified” sorters in the Finder view window.
	Output files are sorted in depth order, then in order they were in on the page.	
	(ie. main page, main page links in order of appearance, then subpage links in order of parent page appearance 
	then order of appearance on parent page)
	If multiple pages have the same name, they will be renamed ~~~(#) where ~~~ is their original name
 	and # is the identifier in order of when they were printed.
	If multiple resources have the same name, the program assumes that they are the same resource, and only prints
	the first one.

Options:
	There are six options near the top of the WebToPDF.py file that can be changed:
		[NOTE: Capitalization is needed for False and True]
		beQuiet: Currently set to False. Change the False to True to prevent the printing of statistics 
			(such as percent done, number to print, number of results, depth, link name, and times).
		[NOTE: The times displayed are the times from the start, not the lengths of each process.]
		killLinks: Currently set to True. This determines whether or not links on the pdf are active. Change to 
		False to enable the links.
		[NOTE: When active, the links will point to the moodle server, not to the other pdfs.]
		getUsers: Currently set to False. Change the False to True to get the profiles of students in the class.	
		getStudents: Currently set to False. If this is false, it changes all of the students’ names in forums to
			“Student Name” and changes the link to the Moodle home page.
		[NOTE: This only works for names that Moodle generates. If the professor types in the student's name, the
		program will not be able to remove it.]
		getResources: Currently set to True. Change to False to prevent downloading of assets.
		getForums: Currently set to True. Change to False to prevent getting pdfs of the forum links on the websites.
		getWorkshops: Currently set to False. Change to True to get workshop pages.
		[NOTE: The program is unable to remove student names from workshop pages, and these pages often show grades.]
		getQuestionnaires: Currently set to False. Change to True to get questionnaire pages.
		getFeedback: Currently set to False. Change to True to get feedback pages.
		getScheduler: Currently set to False. Change to True to get scheduling pages.
		onlyOne: Currently set to True. When true, only one large pdf is printed with the contents of the entire course.
			If set to false, each page on the course gets its own pdf.
		goToDepth: Currently set to 2. This defines how deep the program goes into the site.
			1 = Course Main Page and Links from the Main Page
			2 = Course Main Page, Links from the Main Page, and Links from those Links
		loginPage: Currently set to 'https://moodle2013-14.carleton.edu/login/index.php'. This is the page the 
			program goes to to login. This needs to be set to the same year as the moodleDomain.
		moodleDomain: Currently set to ‘https://moodle2013-14.carleton.edu/'. This is the domain that the
			program uses to determine the validity of links.

	There is also an array called options that has multiple values that determine the page layout of the printed pdf’s.
	The first five can be changed to alter the size and margins of the printed pdf’s, but the last three should be left
	alone.