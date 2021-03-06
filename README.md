
# Description:
 This script is used primarily to inject an event listener on a reddit.com/* page to enable specified keyboard shortcuts in lieu of the mouse. The list of commands is detailed below. This script assumes the use of "vanilla" reddit (no extensions, like RES), jQuery (which should be enabled normally on reddit), and 10 articles per page for maximum efficiency

# Changes:
* 4.1: Added numbering to root comments, for easy navigation after returning to the comments (for use with /r)
* 4.0: Version 4.0 is here!! Added a method for scrolling between root comments and created a better way of determining which comments are root comments.
* 3.999: Added a root comment search mode. The method for selecting root comment is not yet perfect, and root searching may not work on certain subreddits, based on the current test. This build may be buggy.
* 3.99.5: Added domain searching in search mode
* 3.99: Added up/downvoting for selected comments
* 3.9: Added rudimentary commenting commands
* 3.1: Changed the method of selecting the "next page" link to be more robust
* 3.0: Consolidated clicking text and image links into one statement, as well as making it more jQuery friendly. This update doesn't add or update any visible features, but restructures the code enough to warrant an update
* 2.12: Added a border for selected elements to distinguish them in subreddits that don't display the rank number
* 2.11: Updated input check to better determine if the user is focused on a textbox or not.
* 2.10: Added a command to go back to the link in the article on the comments page for that article
* 2.9: Added commands for the user's profile page. See below in the list of commands
* 2.8: enabled more interactivity with selected articles
* 2.7.1: give search boxes arbitarily high z-indicies, so they are placed on top of the multireddits tab, if open
* 2.7: disabled default action for the 's' key after a '\', for search mode. Before it automatically put an s in the search box. disabling the default action for an event fixes this.
* 2.6: Added enhanced search functions; press / to enter search mode, then press + or u to pull up an input box to go to a subreddit or user page, respectively, or s to focus on the search box

# To Fix:
  Establish a solid method of determining which comments are root comments


# To Do (potentially)
* Add commands for navigating comment trees. ($(".comment") grabs a div for each individual class. While rudimentary, this is a start for comment support)
* Add support for more than 12 multireddits
* Restructure the code so its not so directly linear (functions, better structuring, etc)


# List of commands:
* 0-9: The first time the number keys are pressed, it "selects" the chosen article, which enables further action:
	* Pressing that number again will click on the article. Pressing a new number will select a new article
	* PageUp/PageDown will up/down vote the article, respectively
	* c will go to the comments for that article
	* s will save the article
	* r will go to the subreddit the article is from, if on the home page or a multi page
* left cursor key: go back 1 page in your history. e.preventDefault is called in this event to prevent Alt+Left Cursor Key to go back two pages. There may be a bug regarding up/down voting and this method; not sure if reddit's servers take a while to register votes, or if going back "erases" the memory of the vote
* right cursor key: click on the "next >" button, advancing to the next set of articles.
* /: the forward slash will enable "search mode". after pressing /, pressing:
	* + will create and focus on a textbox at the bottom of the page. enter a subreddit name here and press enter to go there. The plus sign in the text box is simply there for aestetic purposes. the function ignores it when processing the link.
	* u will create and focus on a text box at the bottom of the page. enter a user's name here and press enter to go to their profile
	* s will focus on the page's search box.
	* c, on a comments page, will create and focus on a text box at the bottom of the page. Entering a number here and pressing enter will scroll the page so that comment is visible at the top of the screen. This will also 'save' the selected comment for further commands, much like selecting articles
	* d will create and focus on a textbox at the bottom of the page. Entering a url and pressing enter will return articles from that domain
	* r, on a comments page, acts very similar to the regular comments search, but only for root comments.
* After returning from a visited link, the commands used for a selected article will work as well. Note selected articles take prescidence, i.e. if you have a last-visited article and a different selected article, pressing c will open the comments for the selected article, not the last visited.
* h: go to https://www.reddit.com
* m: go to your profile
* i: go to your inbox
* r: go to /r/random
* Function keys: If on the home page or a multireddit page, pressing the nth function key will take you to the nth multireddit in your multireddits on the left bar. Currently only supports 12 multireddits (for 12 function keys).
* If the user is on a profile page, a new set of options is opened to them (note these actions take them to *their* respective pages):
	* o: overview: go to the main profile page
	* c: comments: go to the comments page
	* g: gilded: go to the gilded page
	* u: upvoted: go to the upvoted page
	* d: downvoted: go to the downvoted page
	* j: hidden: go to the hidden page. j is used instead of h because h already is used to go to the home page, and this takes precidence.
	* s u: submitted: go to the submitted page. This works by pushing s, then u
	* s a: saved: go to the saved page.
* l: if on the comments page for an article, pressing l will go to the link in the article
* w: if on a comments page, select and scroll to the first root comment if none are selectd, or scroll to the root comment above the currently selected root comment
* s: if on a comment page, select and scroll to the first root comment if none are selected, or scroll to the root comment below the currently selected root comment



# Comment Trees
The simplest comment tree looks like the following:

	Root Comment:
	|
	-	Child Comment:
		|
		-	Grandchild Comment:
			| 
			-	GreatGrandchild Comment:
				|
				...
etc.
However, this is not the only tree structure. For example:

	Root Comment:
	|
	-	Child Comment 1:
		|
		-
			Grandchild Comment:
		Child Comment 2: 
		|
		-	Grandchild Comment:
		...

etc. This throws off what comment is selected, though it shows the proper parent comment. USE ID of the anchor to navigate properly!!
