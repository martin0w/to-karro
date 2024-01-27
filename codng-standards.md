# Karro kodningstips


Code formatting
===

1. Code formatting - generally OK.
2. Line breaks and empty lines

Tip: use emty lines more generously, especially when mixing PHP and HTML

3. Indentation: Your indentation in PHP and JS is good.
Tip: use very structured indentation in HTML too, at least with all block tags "down to the <p>-level".

3. Be extra careful with indentation when mixing PHP and HTML.
Example:
`    <select name="room-type" id="room-type" disabled>
        <?php
        foreach ($rooms as $key => $room) { ?>
            <option value="<?= $room['id']; ?>" <?= ($room['id'] == $selectedRoomId) ? 'selected' : ''; ?>><?= $room['comfort_level']; ?></option>
        <?php

        } ?>
    </select>
`
Should be (note matching indentation of opening and closing elements + use of whitespace)
    <select name="room-type" id="room-type" disabled>
        <?php

        foreach ($rooms as $key => $room) { 

        	?>
            <option value="<?= $room['id']; ?>" <?= ($room['id'] == $selectedRoomId) ? 'selected' : ''; ?>>
            	<?= $room['comfort_level']; ?>
            </option>
        	<?php

        } 

        ?>
    </select>



Comments
===

* Comment density - Generally OK. 
   Tip: make it to a habit to comment every 5 lines of PHP/JS code.

* At the beginning of each PHP and JS file, place a comment describing clearly the purpose of the file.

Example:

<?php

/**
 * functions_social_media.php
 * This file contains functions supporting integration with social media such as Facebook.
 */

 ...


* Before each function, place a larger comment describing clearly what the function does, 
  the meaning of each input parameter and what the funciton returns. 

3. Tip: You can use formalized PHP multi-line comments and PHPDOC notation

/**
 * This is a
 * multi-line
 * comment
 */

Example: 

    /**
     * Display a post reaction button (like "clap"), complete with an JS & AJAX handler.
     * Supports multiple clicks to raise the value, plus delayed Ajax save.
     *
     * @param array $args key-value pairs containing reaction data. 
     *              Example: array ( 'object_type' => 'post', 'object_id' => 1234 )
     * @param string $btn_label html label of the button.
     * @return void
     */    
    function post_reaction_button ( $args, $btn_label ) {
    	...
    }


Structure
===

* Break down *whatever code you can* into reusable functions. For example:

Instead of:

        <section class="rooms">
            <?php
            foreach ($rooms as $key => $room) {
                $imgUrl = 'images/' . $room['comfort_level'] . '.png'; ?>
                <div class="room-info">
                    <img class="room-img" src="<?= $imgUrl; ?>" alt="<?= $room['comfort_level']; ?>">
                    <h3><?= $room['comfort_level']; ?></h3>
                    <p><?= $room['description']; ?></p>
                    <p>Room rate: <span class="room-price"><?= $room['price']; ?></span> USD</p>
                </div>
            <?php
            } ?>
        </section>

Use:

        <section class="rooms">
            <?php

            	echo displayRoomsList( $rooms );

			?>
        </section>

* Group your functions into logically named files and/or namespaces and/or classes.
  Tip: The structure and naming of files should allow the reader to get initial understanding
  of what your application does.
  
  Your overall structure of code should be:

  <my application>
  	<main application file>
  		<other files, reflecting the logical structure of the application>
  			<within a file, a header comment describing what the file contains>
  				<lits of functions. for each function, a header comment describing what the funciton does>
  					<commented code for each function>

	Tip: Outline your file and class/function structure at a piece of paper before beginning. This will set you on the right direction wrt how to structure your solution top-down.

* Be consistent and balanced in how you mix PHP and HTML. In a function, you could use either echo statements or HTML blocks to output your HTML, but there can be very many echo() statements. In HTML, you can use <?php ?> tags to inject PHP, but there should not be too many such tags. PHP allows you to do pretty much everything so you need to have discipline to maintain readability of code.

* Break out repeating HTML page elements into functions or sub-files.
  Typically, each page of your website will follow the same wireframe with header, navigation, maybe sidebar, content area, footer. Repeateble elements such as header, footer, navigation should be lifted out as reusable functions and/or include files.


Naming conventions
===

* Be very, very consistent with how you spell/underscore/capitalize all the variables, functions, classes, tables, columns.
There are accepted PHP/JS/SQL conventions that you should always follow, plus your own personal standards that you should think through and apply n to of that.
https://gist.github.com/ryansechrest/8138375#7-variables

* No variable names are too long, but they can be too short. Use long, descriptive names.





File names
===

* Think through your file structure up-front. Also, think rhough your file namings up-front.
Example:

/lib/
	/classes/
		ClassSettings.php
		ClassRoom.php
		ClassClaendar.php
		ClassBooking.php
	/functions/
		functions_utilities.php
		functions_math.php
		functions_booking.php




Safe programming
===

* Whenever you allocate a global resource dynamically, make sure to explicitly release it when it is no longer needed.
  In PHP, much of this is handles automatically for you, but keep an eye of it. A good example is $_SESSION variables 
  Forgetting to deallocate a variable leads can lead unpredictable, difficult errors downstream.
  For example: 
  		$_SESSION['one'] = 'something';
  		...
  		unset ($_SESSION['one']);

  Tip: develop it as a habit. Whenever you declare or start uding a variable, think for a second: "How is this variable going to end?". For example, if this is a local variable within a function, it's going to die when the function ends - no further action necessary. But if you assign a value to a $global, you might explicitly want to restore that $global to its previous state to avoid interfering with any other functions.
