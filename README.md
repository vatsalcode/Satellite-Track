# Satellite-Track

Track orbiting objects (such as the International Space Station) in your terminal!



Requires Python 3.3+ and a terminal with 256 colors. A black background is highly recommended.

.. code-block::

	pip3 install termtrack
	termtrack -figmntxo 1 iss

.. code-block::

	Usage: termtrack [OPTIONS] [SATELLITE]

	  Shows a world map tracking SATELLITE. Valid values for SATELLITE are
	  numbers from http://www.celestrak.com/NORAD/elements/master.php (for
	  your convenience, a number of aliases have been provided).

	  Example satellite aliases (find more with --aliases):
	      hubble          Hubble Space Telescope
	      iss             International Space Station

	  Hotkeys:
	      a       Toggle apsides markers
	      c       Toggle next-orbit coverage overlay
	      d       Toggle ascent/descent markers
	      f       Toggle footprint (satellite horizon)
	      g       Toggle latitude/longitude grid
	      i       Toggle info panels
	      n       Toggle night shading
	      o       Cycle through drawing 0-3 next orbits
	      p       Pause/resume
	      q       Quit
	      r       Reset plotted time to current


How Stuff Works
===============

To draw the map, TermTrack will look at a shapefile from `Natural Earth <http://www.naturalearthdata.com>`_ in order to find coordinates that are within a landmass. While computationally expensive, this method yields the most accurate and good-looking maps at all terminal sizes. To determine the color of each pixel, a relatively low-resolution and low-quality JPEG image is used. If you look at the image (``termtrack/data/earth.jpg``), you'll notice it has green oceans. This is to ensure that ocean blue will not spill over into coastal areas during downsampling. Same goes for the expanded white coast of Antarctica. Finally, the image has been tuned to produce good-looking colors against a black background. The resolution and quality of the image is not really a concern since we do not need maximum per-pixel precision to make the Sahara appear yellow. After computing land/ocean status and land color, this information is cached in ``~/.termtrack_map_cache``, so it will not have to be rendered again for the current terminal size.

For Mars and the Moon there is no shapefile to read and the entire area is colored according to similar JPEG color maps.

Night shading for each pixel is done by looking at the Sun's elevation and shifting the color of the pixel towards blue accordingly. Twilight starts when the Sun is 18° below the horizon and ends when it has risen to 0°.

Satellite locations are derived from `TLE <https://en.wikipedia.org/wiki/Two-line_element_set>`_ data downloaded from `CelesTrak <https://celestrak.com/>`_. The data is fed into pyephem where the current position of the satellite is computed using `SGP4 <https://en.wikipedia.org/wiki/Simplified_perturbations_models>`_. Most of the data you see in the info panels is provided by pyephem, but the apsides' locations as well as the satellite footprint outline are computed by TermTrack itself.


