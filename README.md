google_map_markers
==================

Java code to divide android map into grids and put markers. 


/*
CODE TO DIVIDE MAP INTO GRIDS AND PUT MARKERS ON MAP. ONE MARKER PER GRID.

TO DEFINE:
k: gridsize level (from 1 to 10)
sla, slo: start of grid (lat, long)
ela, elo: end of grid

ARGUMENTS:
zoomlevel
ArrayList<MarkerOptions> marker options to be put on map

LOGIC:
Divide map into grids and then traverse the positions to put. Combine them if they lie in same grid.
*/


	private void arrangeMarkers(float zoomlevel, ArrayList<MarkerOptions> markerOptionsList){
  		ArrayList<MarkerOptions> moList;
		
			moList = new ArrayList<MarkerOptions>();
			long gridsize = K*1000000/(int)zoomlevel;
			
			//Traverse Grid
			long i;
			i=(long)(sla*10000000);
			long j,los;
			j=los=(long)(slo*10000000);
			long lae=(long)(ela*10000000);
			long loe=(long)(elo*10000000);
			int nl=(int)((loe-los)/gridsize);
			//Log.d(TAG, "No of longs : " + Integer.toString(nl));
			
			//Log.d(TAG, "slat:slng:elat:elng"+Long.toString(las)+":"+Long.toString(los)+":"+Long.toString(lae)+":"+Long.toString(loe)+":");
			while(i<=lae){
				
				while(j<=loe){
					moList.add(null);
					double lt = (double)(i+(gridsize/2))/10000000;
					double lg = (double)(j+(gridsize/2))/10000000;
					placeStrings.add(new LatLng(lt,lg));
					j=j+gridsize;
				}
				j=los;
				i=i+gridsize;
			}

			/*KEEP COUNT OF NO OF PLACES PER GRID*/
			int placeCnt[] = new int[moList.size()];
			for(MarkerOptions m: markerOptionsList){
				LatLng l = m.getPosition();
				int lats = (int)((long)((l.latitude-sla)*10000000)/gridsize);
				int lngs = (int)((long)((l.longitude-slo)*10000000)/gridsize);
				int index = lats*nl + lngs;
				//Log.d(TAG, "Index : " + Integer.toString(index)+" lats : "+Integer.toString(lats) + " longs : " + Integer.toString(lngs));
				placeCnt[index]++;
				MarkerOptions nm = new MarkerOptions().position(new LatLng(l.latitude, l.longitude));
				
				if(placeCnt[index]>1){
					nm.title("Area with "+Integer.toString(placeCnt[index]-1)+" more places");
				}else{
					nm.title(m.getTitle());
				}
				moList.set(index, nm);
			}
		
			//USE moList to put markers on MAP.
	}
