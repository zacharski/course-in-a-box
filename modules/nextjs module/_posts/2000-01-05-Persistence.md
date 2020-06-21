## Persistance

<iframe width="560" height="315" src="https://www.youtube.com/embed/SC8b1bbB07U" frameborder="0" allowfullscreen></iframe>



### Stateless Protocol
In a stateless protocol no information is retained on the server between requests. In effect, there is no memory between requests. For example, by default if we have a login post request, a user can log in, but the server doesn't remember that the person logged in. 

### Sessions
The concept of a session either on the server or client allows us to maintain state or memory. There are many good methods to use for both sessions and authentication. in the first video we will look at a very simple solution that may not be the best option for most applications but offers a good introduction to this topic.

### Cookies
Cookies let you store user information in web pages. They are data stored in small text files in a user's browser and were designed to solve the problem of remembering information about users. For example, when a user visits a web page we can store their username in a cookie. The next time the user visits the page it can remember the name. 

Cookies are stored in key, value format.

### js-cookie library

We are going to use the js-cookie node module on the client side. The main functions are

	set(key, value);
	jsCookie.set("screenname", this.state.name); // example
	
	get(key);
	jsCookie.get("screenname");                  // example
	
	remove(key);
	jsCookie.remove("screenname");               // example
	

## EXAMPLE

### login page

Suppose we have a login page that has a render method:

    render() {
      const that = this;
      jsCookie.remove("screenname");
      return (
        <Layout
          style={{ margin: "auto auto", width: "600px", textAlign: "center" }}
        >
          <h2>Login</h2>
          Username:
          <input
            type="text"
            className="text-style"
            value={this.state.username}
            onChange={this.handleUserUpdate.bind(this)}
          />
          <br /> <br />
          Password:
          <input
            type="password"
            className="text-style"
            value={this.state.password}
            onChange={this.handlePasswordUpdate.bind(this)}
          />
          <br />
          <br />
          <div onClick={this.handleSearch.bind(that)} className="button-style">
            Submit
          </div>
          <br /> <br />

You can see that we have an input field of type text for the username and an input field of type password for the password field. For both we have an onChange handler that stores the associated values in this.state. When a user clicks on submit, the handleSearch method executes:

	import jsCookie from "js-cookie";

    async handleSearch(evt) {
      const loggedInUser = await getLogin({
        username: this.state.username,
        password: this.state.password
      });
      this.setState({ loggedInUser });
      if (loggedInUser.status == "success") {

        jsCookie.set("screenname", loggedInUser.screenname);
        Router.replace("/search");
      }
    }

You can see that after we get the information back from the server, we save the screenname in a cookie and then go to the search page.

### the Header.js file

The next file we edited was 

### Header.js

We added the cookie to the header file:

    export default function Header() {
      return (
        <div>
          <Link href="/">
            <a style={linkStyle}>Home</a>
          </Link>
          <Link href="/search">
            <a style={linkStyle}>Find A Campground</a>
          </Link>{ jsCookie.get("screenname") ? 
    <Link href="/logout">
            <a style={linkStyle}>Logout</a>
          </Link> 

           :
          <Link href="/login">
            <a style={linkStyle}>Login/Register</a>
          </Link> }
          {jsCookie.get("screenname")}
        </div>
      );
    }

Here you can see we use the cookie to display the screen name of the user `{jsCookie.get("screenname")}`. And we have a conditional that either displays Login or Logout: `{ jsCookie.get("screenname") ? ...`

### Logout
Finally we added a logout page that included:

      constructor(props) {
        super(props);
        jsCookie.remove('screenname');
      }

      
      componentDidMount(){
         Router.replace("/search");
      }

