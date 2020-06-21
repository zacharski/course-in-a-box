## Activity Prompts
<br/><br/>
## Next, React, Isomorphic Fetch

<iframe width="560" height="315" src="https://www.youtube.com/embed/NGlpKEKXLhc" frameborder="0" allowfullscreen></iframe>

### A review of GET and handling POST

<iframe width="560" height="315" src="https://www.youtube.com/embed/pcemV-Xe0So" frameborder="0" allowfullscreen></iframe>


### Overview
In this video we are going to be learning about Next.js and React.
We are going to be writing front end code. Code that displays and runs on a user's browser.
Our goal isn't to become fluent in front end coding. Our goal is to be able to hack up a front end to our web application. 

In other words, I am not going to be explaining every bit of code that I present. I am hoping to explain enough that you can mock up a user interface for an idea you have for a web application



### Github Repository for the Template

	https://github.com/zacharski/next-template.git

You can install and run this code with the following

	git clone https://github.com/zacharski/next-template.git
	cd next-template
	npm install
	npm run dev

Now we are going to add a search box and button that will get information about a user from github. Once it gets that information it will display it.

### Step 1.

Add state to constructor

	this.state={search: 'notwaldorf'}
	
And after h1 we will add the text box and button div

         <p><input type='text' value={this.state.search}  /></p>
         <div className="button-style">Submit</div>
 
and add to the css section the button style information:

          .button-style {
            margin: auto auto;
            cursor: pointer;
            background-color: #4633FF;  
            color: #ffffff; 
            width: 100px;
            font-family: "Arial";
          }


We can see the results on our webpage.

### Step 2

When we type in a name in the search we would like to save it so we can use it when we hit the search button

For this we will add a handler after constructor:

  	handleUpdate(evt){
    	this.setState({search: evt.target.value})
 	 }

then add to search box

	onChange={this.handleUpdate.bind(this)}	
	
Now if we go to our webpage and look at the component tab of the React development tools we will see the variable `search` dynamically change when we change the input text.


### Step 3 - connecting to github

	https://api.github.com/users/notwaldorf
	
create file `/lib/utils.js`

	require("isomorphic-fetch");


	function getProfile(username) {
	  return fetch(`https://api.github.com/users/${username}`).then(function(resp) {
	    return resp.json();
	  });
	}

	function handleError(error) {
	  console.warn(error);
	  return null;
	}

	module.exports = {
	  getInfo: function(user) {
	    return getProfile(user).catch(handleError);
	  }
	};


add to index

	import {getInfo} from '../lib/utils'
	
	
    async handleSearch(evt) {
      const user = await getInfo(this.state.search);
      console.log(user)
      this.setState({user})
  }
  
finally 

	onClick={this.handleSearch.bind(this)} 




### 4. display the results


        {this.state.user ? <div> 
        <h3>{this.state.user.name}</h3>
              <img style={{maxWidth: '100px', maxHeight: '100px'}} src={this.state.user.avatar_url} /><br/>

          </div> : null }
**show**

then add


        {this.state.user ? <div> 
        <h3>{this.state.user.name}</h3>
              <img style={{maxWidth: '100px', maxHeight: '100px'}} src={this.state.user.avatar_url} /><br/>
              <h4>{this.state.user.company}</h4>
              <p>{this.state.user.bio}</p>
              <p><b>followers</b>: {this.state.user.followers}</p>
          </div> : null }
