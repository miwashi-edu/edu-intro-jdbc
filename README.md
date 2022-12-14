# intro-jdbc

> Nu gör vi en något komplexare lösning. Detta gör man oftast om man vill att programmet fortsätter även om något går fel.

```java
package se.iths;

import java.sql.*;
import java.util.ArrayList;
import java.util.Collection;
import static se.iths.Constants.*;

public class App {
  
  public static void main(String[] args)  {
      App app = new App();
      app.load();
  }

  private void load(){
    Collection<Artist> artists = loadArtists();
    for(Artist artist: artists){
      System.out.println(artist);
    }
  }
  
  private Collection<Artist>  loadArtists() {
    Collection<Artist> artists = new ArrayList<>();
    Connection con = null;
    ResultSet rs = null;
    try {
      con = con = DriverManager.getConnection(JDBC_CONNECTION, JDBC_USER, JDBC_PASSWORD);
      rs = con.createStatement().executeQuery(SQL_SELECT_ALL_ARTISTS);
      while(rs.next()){
        long id = rs.getLong(SQL_COL_ARTIST_ID);
        String name = rs.getString(SQL_COL_ARTIST_NAME);
        Artist artist = new Artist(id, name);
        artists.add(artist);
        Collection<Album> albums = loadAlbums(con, artist.getId());
        for(Album album : albums) {
          artist.add(album);
        }
      }
    } catch (SQLException e) {
      System.err.println(String.format("Fel vid läsning av databas %s", e.toString()));
    } finally {
      try {
        rs.close();
      }catch (Exception ignore){}
      try {
        con.close();
      }catch (Exception ignore){}
    }
    return artists;
  }

  private Collection<Album> loadAlbums(Connection con, long artistId) {
    Collection<Album> albums = new ArrayList<>();
    ResultSet rs = null;
    PreparedStatement stmt = null;
    try {
      stmt = con.prepareStatement("SELECT AlbumId, Title FROM Album WHERE ArtistID = ?");
      stmt.setLong(1, artistId);//Sätt värde för första frågetecknet
      rs = stmt.executeQuery();
      while(rs.next()){
        long id = rs.getLong(SQL_COL_ALBUM_ID);
        String title = rs.getString(SQL_COL_ALBUM_TITLE);
        Album album = new Album(id, title);
        albums.add(album);
      }
    } catch (SQLException e) {
      System.err.println(String.format("Fel vid läsning av Album %s", e.toString()));
    } finally {
      try {
        rs.close();
      } catch (Exception ignore) {
      }

      try {
        stmt.close();
      } catch (Exception ignore) {
      }
    }
    return albums;
  }
}
```
