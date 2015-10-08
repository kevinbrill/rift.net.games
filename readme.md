##Rift.Net.Games

###About

rift.net.games is a managed wrapper around the scratch game functionality of [Trion World's](http://www.trionworlds.com) MMO [Rift](http://www.riftgame.com).

For details on the parent project, please refer to [Rift.Net](http://github.com/kevinbrill/rift.net)

###Features

Currently, the following features of the API are supported:

* Viewing a list of all scratch card games that can be played
* Viewing an account's game status, including the number of games that can currently be played
* Play any of the scratch card games

###Install
From the package manager console, type in

    Install-Package rift.net.games

###Using

The mobile scratch games are also surfaced through the Rift API, and have been wrapped by Rift.net as well.  There are two main components to the games:

* The account information, including the maximum number of points available, the current number of points available, and the time to the next game
* A list of games that are available

The games are wrapped in the ScratchCardClient class, which is constructed in the same fashion as the RiftSecuredClient.
	
	// Create a new session factory
	var sessionFactory = new SessionFactory ();

	// Login using provided username and password
	var session = sessionFactory.Login (username, password);
	
	// Create a new secured client 
	var client = new ScratchGameClient (session);
	
	// Get the user's game information
	var info = client.GetAccountGameInfo();
	
	Debug.WriteLine( info.MaximumPoints );
	Debug.WriteLine( info.AvailablePoints );
	Debug.WriteLine( info.SecondsUntilNextPoint );
	Debug.WriteLine( info.Games.Count );
	
	// List all games
	var games = client.ListGames();
	
	foreach( var game in games ) 
	{
		Debug.WriteLine( game.Name );
		Debug.WriteLine( game.Description );
	}
	
Seeing the available games is fun, but playing them is even better.  Once you have a secured client, you can play games this way:

	// Who doesn't love Shinies?
	var shinies = games.FirstOrDefault( x => x.Name == "Shinies" );
	
	try
	{
		// Would you like to play a game?
		var prizes = client.Play( shinies, charactedId );
		
		if( prizes.Count == 0 ) 
		{
			Debug.WriteLine("Sorry you didn't win");
		}
		else
		{
			foreach( var prize in prizes )
			{
				Debug.WriteLine( string.Format("You won {0} copies of {1}!", prize.Quantity, prize.Name);
			}
		}
	}
	catch( NoCardsAvailableException ex )
	{
		Debug.WriteLine( "No cards are available" );
	}
	catch( InvalidGameException ex )
	{
		Debug.WriteLine( "That's an invalid game" );
	}
	
The actual playing of a game and providing rewards is all done by Trion on their servers, so this wrapper doesn't allow you to cheat or game the system (which was never the intent anyways).

###Contributing
If there's a feature that you'd like to see, you can create an issue.  If there's something that you've fixed or improved, then create a pull request.  Pull requests are great!

You can reach me in game at **Bruun@Wolfsbane**.  If you're looking for a home in Telara, check out **Grievance** at [http://grievancegaming.org/](http://grievancegaming.org/)

##Legal Stuff
I am in no way affiliated or employed by Trion Worlds, nor do I receive any compensation from them for my work on this project or any other project.

I am also not responsible for any damages that are incurred by using this library.  Any usage should be done at your own risk.