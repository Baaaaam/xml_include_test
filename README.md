# xml_include_test
I wanted to include an xml file in my cyclus input, in order to simply a little bit it...
I tried the way described in http://fuelcycle.org/user/writing_input.html.

Unfortunately it did not work as is : apparently XMLinclude is only able to include correct XML file in your input and 2 block input is not a correct XML file.

I have built some example to show the problem and the solution I found: you can find them this repo.

The **website_main.xml** follow the instruction of the website.
to check the syntax I used : *xmllint -xinclude main.xml* ( which shows the same error as using cyclus on a real input file)

as you can see the example of the website did not work (website_main.xml):

    website_include.xml:10: parser error : Extra content at the end of the document
    <prototype>
    ^
    website_main.xml:2: element include: XInclude error : could not load website_include.xml, and no fallback was found
    <?xml version="1.0"?>
    <simulation xmlns:xi="http://www.w3.org/2001/XInclude">
    <xi:include href="website_include.xml"/>
    </simulation>



but when removing the second block of the include (website_main_one_block_include.xml):
it works!

a proper way to deal with it is to add a master block in the included xml file (such as <XXX> ... </XXX>) and to change the insertion command :

    <xi:include href="working_include.xml" xpointer="xpointer(//XXX/*)"/>
    
*(see working ... .xml files...)*

the addition of xpointer="xpointer(//XXX/*)" will allows to include all blocks in the <XXX> </XXX> block but without including the master block <XXX>
