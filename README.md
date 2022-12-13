# intro-jdbc

> Den här lösningen är snabbast, då det bara blir en anslutning mot databasen och vi använder databasen till att slå ihop Artist och Album.

> Vi väljer återigen en enklare felhantering.

```java
package se.iths;

import java.sql.*;
import java.util.ArrayList;
import java.util.Collection;
import static se.iths.Constants.*;

public class App {

  public static void main(String[] args)  {
    App app = new App();
    try {
      app.load();
    } catch (SQLException e) {
      System.err.println(String.format("Något gick fel vid inläsning av databas! (%s)", e.toString()));
    }
  }

  private void load() throws SQLException {
    Collection<Artist> artists = loadArtists();
    for(Artist artist: artists){
      System.out.println(artist);
    }
  }
  
  private Collection<Artist>  loadArtists() throws SQLException {
    Collection<Artist> artists = new ArrayList<>();
    Connection con = con = DriverManager.getConnection(JDBC_CONNECTION, JDBC_USER, JDBC_PASSWORD);
    ResultSet rs = con.createStatement().executeQuery(SQL_SELECT_ALL_ARTISTS_WITH_ALBUMS);
    long oldId = -1;
    Artist artist = null;
    while(rs.next()){
      long id = rs.getLong(SQL_COL_ARTIST_ID);
      String name = rs.getString(SQL_COL_ARTIST_NAME);
      long albumId = rs.getLong(SQL_COL_ALBUM_ID);
      String title = rs.getString(SQL_COL_ALBUM_TITLE);
      if(id!=oldId) {
        artist = new Artist(id, name);
        artists.add(artist);
      }
      artist.add(new Album(albumId, title));
    }
    rs.close();
    con.close();
    return artists;
  }
}
````
