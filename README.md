Download Link: https://assignmentchef.com/product/solved-ee312-project-3-string-adt
<br>
In this project, we are creating an Abstract Data Type (ADT) to improve upon the standard C way of representing strings. We will use malloc and free, and get some practice with pointers, structs, and macros.

<h1>On Strings</h1>

Strings in C are stored simply as an array (buffer) of ASCII-encoded characters with a zero (null character) on the end. This works well, but has some drawbacks. Importantly, strings are a common source of buffer overflow errors, a serious security vulnerability that happens when programmers forget how large the string’s buffer actually is when writing to it.

Our new ADT will still store the actual string in the basically the same way as before — however, it will also store some extra information as well — specifically, the length (number of characters) and the capacity (how many characters can it hold before buffer overflow would result). As some amount of extra safety, in the four bytes immediately following the string, we will store a not-sorandom looking byte sequence that we can check to confirm that nothing bad has happened (yet).

<h1>On UTStrings</h1>

You are to implement each of the functions declared inside String.h. You must use (without modifying) the UTString struct that is defined inside String.h. This struct consists of the following parts:

<ul>

 <li><strong>length </strong>– The length of the string. This is the number of characters in the string, and does not include the null character, nor anything after it. Keep in mind that the length of a string may be shorter than the buffer in which the string is stored. For example, if we had the string “hello”, then the length would be 5 (one for each useful character), regardless of the size of the buffer.</li>

 <li><strong>capacity </strong>– The length of the longest string that can be stored in the buffer. For example, if we had a string with a buffer of length 20, then capacity would be 15 (20, minus the null character and check), regardless of the length of the string.</li>

 <li><strong>string </strong>– A pointer to the buffer where the string is stored. It must be allocated separately from the UTString itself.</li>

 <li><strong>check </strong>–The signature value (~0xdeadbeef) stored after a string. It is not a true member of the struct, but should be in it regardless. We will check the value every time when working with a UTString to make sure that no buffer overflow has occurred yet. If it does not check out correctly, your program should fail an assert and crash immediately. UTString struct:</li>

</ul>

<table width="0">

 <tbody>

  <tr>

   <td width="566">length (int, 4 bytes) = 11</td>

  </tr>

  <tr>

   <td width="566">capacity (int, 4 bytes) = 11 or greater</td>

  </tr>

  <tr>

   <td width="566">string (char*, 4 bytes) pointer to location of buffer on heap of at least capacity</td>

  </tr>

 </tbody>

</table>

The contents of the string buffer for the string “Hello World” (the single quotes and 0x are not shown for brevity):




<table width="0">

 <tbody>

  <tr>

   <td width="35">H</td>

   <td width="38">e</td>

   <td width="37">l</td>

   <td width="42">l</td>

   <td width="36">o</td>

   <td width="65">‘ ‘</td>

   <td width="42">W</td>

   <td width="42">o</td>

   <td width="42">r</td>

   <td width="42">l</td>

   <td width="42">d</td>

   <td width="42"> </td>

   <td width="168">~0xdeadbeef</td>

  </tr>

 </tbody>

</table>




If the buffer is bigger, the bytes after 0xef are unknown.




UTStrings can be used much as an ordinary string. Anyone choosing to use our library should declare variables of type UTString*, and should only use our functions to change the stored string. Some notes:




<ul>

 <li>UTStrings can only be stored on the heap. Declaring a local or global variable of type “UTString” will not work. We can, of course, declare local variables of type “UTString*” (pointers to UTString). We’ll point these pointers at chunks from the heap.</li>

 <li>Our clients (anyone using our library) will only create strings by calling the <em>utstrdup </em> Our clients will not poke around inside our struct, or to mess with the characters inside the array. Our clients will call <em>utstrfree </em>with their UTStrings when they’re done with them.</li>

</ul>




<h1>Goals</h1>

Write the following functions:




<ul>

 <li>UTString* utstrdup(const char* src); — An analog of the C stdlib function <em>strdup</em>. <em>src</em> is a string. Create a UTString on the heap that holds a copy of source, setting the length, capacity, and check appropriately. Return a pointer to the UTString.</li>

</ul>




<ul>

 <li>uint32_t utstrlen(const UTString* src); — An analog of the C stdlib function <em>strlen</em>. <em>src</em> is a UTString. Find and return the length of the string.</li>

</ul>




<ul>

 <li>UTString* utstrcat(UTString* s, const char* suffix); — An analog of the C stdlib function <em>strcat</em>. Append characters to <em>s</em> from <em>suffix</em> until out of capacity or done copying. Do not copy more than can be stored. Do not allocate further space. Do use a null terminator and update the check. Return <em>s</em> after appending.</li>

</ul>




<ul>

 <li>UTString* utstrcpy(UTString* dst, const char* src); — An analog of the C stdlib function <em>strcpy</em>. Should replace characters in <em>dest</em> with characters from <em>src</em> until out of capacity or done copying. Do not copy more than can be stored. Do not allocate further space. Do use a null terminator and update the check. Return <em>dst</em> after copying.</li>

</ul>




<ul>

 <li>void utstrfree(UTString* self); — Used to free a UTString. Similar to the C stdlib function <em>free</em>. Must deallocate both the string buffer and the UTString itself.</li>

</ul>




<ul>

 <li>UTString* utstrrealloc(UTString* s, uint32_t new_capacity); — Used to reallocate space for a UTString. Similar to the C stdlib function <em>realloc</em>. If <em>new_capacity</em> is larger than the current capacity, create a buffer with <em>new_capacity</em> capacity, copy all the old contents, and deallocate the old buffer. Otherwise, do nothing. Either way, return <em>s</em></li>

</ul>




Every UTString passed to any of these functions should have a valid <em>check</em>. If it does not, your program should fail an assert and crash immediately.




Submit all files created or edited for the assignment except testing or debugging files. Submit no more, and no less.

<h1>Testing</h1>

The provided tests are useful, but not very thorough (particularly with regards to utstrcpy). Please write your own tests to verify your own code.




The stage 1 and stage 2 tests are intended to convey how UTStrings work. The stage 3 test is trying to ensure you’ve done nothing silly. If all goes well, this test should run in less than a second and should not have any buffer overflows. Otherwise, you’ve probably done something silly.




The stage 4 test is trying to ensure that you are actually checking things as intended.

<strong> </strong>

We will use valgrind to grade your program on memory leaks.




<h1>Hints</h1>

<ul>

 <li>You could write your own functions within Project3.cpp to avoid repeating code, for modularity, or for debugging.</li>

 <li>Look for off-by-1 errors in setting length, setting capacity, or copying characters.</li>

 <li>Think of valid but unexpected test cases – for instance, appending a string of length 0.</li>

 <li>Avoid writing code that does the same thing many times. If you need to, say, find the length of a regular string several times during the project, you should write a function to do so.</li>

</ul>




<h1>FAQ</h1>

Q: Will null pointer inputs be checked?

A: No.




Q: May we use the standard C string library?

A: Yes, you may include string.h, but only use them with C strings and not UTStrings. The standard functions are not compatible with UTStrings because of the extra data.




Q: How long is a “long time” for stage 3?

A: If it takes more than 2-3 seconds, there’s probably something wrong.




Q: If we have a UTString, how do we find the signature?




A: Use the macro defined at the top of the file.




Example, where str is a valid UTString*: CHECK(str)




Q: When initializing the String struct on the heap from a char*, what should the initial capacity be? A: The capacity should be initialized to the length of the char* in the strdup function, since we need to allocate at least that much space to fit the string.




Q: How do length and capacity work?

A: Length refers to the meaningful length of the utstring. Capacity is the maximum amount of meaningful information the utstring can hold.

The null terminator is not a meaningful piece of information, as every string has one at the end and is just a way to communicate that the string has ended. Neither is the check, for similar reasons.




The length and capacity are numbers in the String struct. These values do not count the null character. However, when allocating space, you do need to allocate 1 byte more than the length, to include the null character (plus the space for the header/metadata, of course). And you have to actually set the character after the last character in the string to null.




Q: What is the ptr-&gt;data arrow for?

A: This is just shorthand for (*ptr).data. We write this a lot, so the arrow saves us some characters. If you want to use * instead, make sure to include the parentheses, or else it will look for data in the pointer and not find anything meaningful.







Q: Do we have to check if malloc() or realloc() fails?

A: For this project you don’t have to check if malloc returns a null pointer.







Q: Do we have to check if we’re given valid UTStrings for functions that operate on UTStrings?




A: Yes. You should #include &lt;assert.h&gt; and use assert() to check if the UTString is valid.




Example:

assert(isOurs(my_string));

This will be the first thing in functions where you need to check, and then you can write the rest of the function afterward as normal.

Q: What about Stage 4?




A: Stage 4 will be tested.




Stage 4 checks if you used assert() properly.




Adding a print function for UTStrings might be useful for debugging. Make sure to use Valgrind to eliminate all memory leaks!




Q: My gcc/g++ is not working on kamek with the Makefile.

A: In order to run the provided Makefile on the LRC servers, please log on to kamek and enter the following command: module initadd gcc

Then log out of the server and log back in.

This will permanently update gcc and g++ on your account on the server so that they will be compatible with our Makefile.




Q: How do I write the ‘signature’ into a character array?

CHECK(s) = SIGNATURE;

where s is a UT string.  Since SIGNATURE is a 32 bit Int, and CHECK(s) is a dereference of an int pointer, this will set the 4 bytes after the null character correctly.





