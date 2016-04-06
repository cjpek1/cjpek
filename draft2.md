# cjpek
/*
 * Morse Code receiver app information:
 *
 * Function: messageFinished(): stops the capturing process
 *
 *	 You can call this function to let the app know that the 
 *	 end-of-transmission signal has been received.
 *
 * -------------------------------------------------------
 *
 * ID: messageField: id of the message text area
 *
 *	 This will be a textarea element where you can display
 *	 the recieved message for the user.
 * 
 * -------------------------------------------------------
 *
 * ID: restartButton: id of the Restart button
 *
 *	 This is a button element.  When clicked this should 
 *	 cause your app to reset its state and begin recieving
 *	 a new message.
 *
 */


// ADD YOUR ADDITIONAL FUNCTIONS AND GLOBAL VARIABLES HERE


/*
 * This function is called once per unit of time with camera image data.
 * 
 * Input : Image Data. An array of integers representing a sequence of pixels.
 *		 Each pixel is representing by four consecutive integer values for 
 *		 the 'red', 'green', 'blue' and 'alpha' values.  See the assignment
 *		 instructions for more details.
 * Output: You should return a boolean denoting whether or not the image is 
 *		 an 'on' (red) signal.
 */

var messageField = document.getElementById("messageField");

var morseTable = {   
    '.-': 'A',
    '-...': 'B',
    '-.-.': 'C',
    '-..':'D' ,
    '.': 'E' ,
    '..-.':'F',
    '--.': 'G' ,
    '....':'H' ,
    '..': 'I' ,
    '.---':'J' ,
    '-.-':'K' ,
    '.-..':'L' ,
    '--':'M',
    '-.' :'N' ,
    '---':'O' ,
    '.--.':'P' ,
    '--.-':'Q' ,
    '.-.':'R',
    '...' :'S' ,
    '-':'T' ,
    '..-':'U',
    '...-':'V' ,
    '.--':'W' ,
    '-..-' :'X' ,
    '-.--':'Y' ,
    '--..':'Z' ,
    '-----':'0' ,
    '.----':'1' ,
    '..---':'2' ,
    '...--':'3' ,
    '....-':'4' ,
    '.....':'5' ,
    '-....':'6',
    '--...':'7' ,
    '---..':'8' ,
    '----.':'9' ,

    // Punctuation
    '.----.' :'\'' ,
    '.-.-.-' :'.', 
    '--..--' :','  ,
    '---...' :':'  ,
    '..--..' :'?'  ,
    '-....-' :'-'  ,
    '-..-.'  :'/'  ,
    '.--.-.' :'@' ,
    '-...-'  :'=' ,
    '-.--.'  :'('  ,
    '-.--.-' :')' ,
    '.-..-.' :'"' ,
    '.-.-.'  :'+' ,
    '...-..-':'$' ,
    '..--.-' :'_'  ,
    '-.-.--' :'!' ,
    
    // Prosigns
    
    '.-.-' :'\n',          // New line
     '...-.-'  : 'SK'         // End-of-Transmission

};
var on = 0, off = 0;
var imageDecodeArray = [];
var letterO = "", display = "", restart = "";
var messageFieldRef = document.getElementById("messageField");

function checkCount(count){
    if (count == 1 || count == 2) {
        return ".";
    }else if (count >= 3) {
        return "-";
    }
};

function convertLetter(letter){
	if (letter.length > 0)  
	{
	for (var prop in morseTable){
        if (letter === prop) {
           return morseTable[prop];
		}
        }
    }
	else
	{
		return "";
	}
};


document.getElementById("restartButton").onclick = restartButtonClicked;

function restartButtonClicked(){
	
	messageField.value = "";
	display = "";
}


/*
 * This function is called once per unit of time with camera image data.
 * 
 * Input : Image Data. An array of integers representing a sequence of pixels.
 *         Each pixel is representing by four consecutive integer values for 
 *         the 'red', 'green', 'blue' and 'alpha' values.  See the assignment
 *         instructions for more details.
 * Output: You should return a boolean denoting whether or not the image is 
 *         an 'on' (red) signal.
 */
function decodeCameraImage(data)
{
	
		if (data[0] > data[2])
		{
			
			on ++;
			off = 0;
			return true;
		}
		else
		{
			if (on > 0)
			{
				letterO += checkCount(on);
			}
			
			off++;
			if (off == 1 || off == 2)
			{
				on = 0; //resetting the value
			} 
			else if (off >= 3 && off < 7)
			{
				if (on > 0);
				{
					var a = convertLetter(letterO);

					if (a === 'SK') 
					{
						messageFinished();
					}
					else if (!(a == ""))
					{
						display += a; 
					}
				

				letterO ="";
				messageField.value = display;
				}

			} 
			else if (off >= 7)
			{
				for (i = 0; i < display.length; i++)
				{
					if (!(display.substr(display.length - 1, 1) == " "))
				{
				//display the text in message field
				display += " "			
				messageField.value = display;
				}
				}
			}
			return false;
		}
}

