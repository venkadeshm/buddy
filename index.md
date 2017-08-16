<html>
<head>
<tittle align="center">Maruthuva vilayaatu</tittle>
<script>
	var movingCoin;
	var movableLocations=new Array();
	var board;
	window.onload=init;
	function init(){
		initBoard();
		makeCoinsMovable("white");
		
		
	}
	//Stroes the current coin positions in an array called "board" 
	function initBoard(){
		board=new Array(8);
			
		for(var i=0;i<board.length;i++){
			board[i]=new Array(8);
		}
		for(var i=0;i<board.length;i++){
			for(var j=0;j<board[i].length;j++){
				if(i<2)
					board[i][j]="white";//white coin
				else if(i>5)
					board[i][j]="black";//black coin
				else
					board[i][j]=null;//empty place
			}
		}
			
	}
	//set a group of coins's (based on color) onclick,onmouseover event properties to corresponding methods 
	function makeCoinsMovable(color){
		var imgArray=document.getElementsByClassName(color);
		for(var i=0;i<imgArray.length;i++){
			imgArray[i].onclick=coinOnClickEventHandler;
			imgArray[i].onmouseover=coinOnMouseOverEventHandler;
			//imgArray[i].onmouseout=coinOnMouseOutEventHandler;
			
		}
	}
	//set a group of coins's (based on color) onclick,onmouseover event properties to null
	function makeCoinsImMovable(color){
		var imgArray=document.getElementsByClassName(color);
		for(var i=0;i<imgArray.length;i++){
			imgArray[i].onclick=null;
			imgArray[i].onmouseover=null;
			
		}
	}
	function stopMouseOverEvent(color){
		var imgArray=document.getElementsByClassName(color);
		for(var i=0;i<imgArray.length;i++){
			imgArray[i].onclick=null;
			imgArray[i].onmouseover=null;
			//imgArray[i].onmouseout=coinOnMouseOutEventHandler;
			
		}
	}
	 
	
	function coinOnClickEventHandler(evntObj){
		makeMovableLocationUnClickable();
		unShowMovableLocations();

		movableLocations=new Array();
		//makeCoinsImMovable(evntObj.target.color);
		movingCoin=evntObj.target;
		findMovableLocations();

		makeMovableLocationClickable();
		showMovableLocations();		
				
	}
	function coinOnMouseOverEventHandler(evntObj){
		/*
		makeMovableLocationUnClickable();
		unShowMovableLocations();
		movableLocations=new Array();
		
		movingCoin=evntObj.target;
		findMovableLocations();
		showMovableLocations();*/
	}



	////------------------------------------------finding Movable positions--------------------------------------////


	function findMovableLocations(){
		
		var coinPosition=idStringToIntArray(movingCoin.parentElement.id);
		if(movingCoin.name=="rook"){
		rookMovablePositions(coinPosition);
		}
		if(movingCoin.name=="knight"){
		knightMovablePositions(coinPosition);
		}
		if(movingCoin.name=="bishop"){
		bishopMovablePositions(coinPosition);
		}
		if(movingCoin.name=="queen"){
		bishopMovablePositions(coinPosition);
		rookMovablePositions(coinPosition);
		}
		if(movingCoin.name=="king"){
		kingMovablePositions(coinPosition);
		}
		if(movingCoin.name=="pawn"){
			if(movingCoin.dataset.color=="white")
				whitePawnMovablePositions(coinPosition);
			if(movingCoin.dataset.color=="black")
				blackPawnMovablePositions(coinPosition);
		}
		
	}


	
	//----------------methods,helper methods to find movable locations of each type of coins---------//
 
	function addPosIntoMovableLcations(i,j){
		var pos;
		pos=concatTwoIntegersToString(i,j);
		movableLocations.push(pos);
	}
        //----------1.White Pawn--------------------------------------------------------------------//
	function whitePawnMovablePositions(coinPosition){
		var rowPos=coinPosition[0];
		var colPos=coinPosition[1];
		var pos;
		var i,j;
		i=rowPos+1;
		j=colPos;
		if(i<=7){
			if(!isCoinInThisLocation(i,j)){
				addPosIntoMovableLcations(i,j);
			}
		}
		if(rowPos==1){
			i=rowPos+2;
			if(i<=7){
				if(!isCoinInThisLocation(i,j)){
					addPosIntoMovableLcations(i,j);
				}
			}
			
		}
		i=rowPos+1;
		j=colPos-1;
		if(i<=7 && j>=0 && isBlackCoinInThisLocation(i,j)){
			addPosIntoMovableLcations(i,j);
		}
		i=rowPos+1;
		j=colPos+1;
		if(i<=7 && j<=7 && isBlackCoinInThisLocation(i,j)){
			addPosIntoMovableLcations(i,j);
		}
	}

	//----------2.Black Pawn-------------------------------------------------------------------//
	function blackPawnMovablePositions(coinPosition){
		var rowPos=coinPosition[0];
		var colPos=coinPosition[1];
		var pos;
		var i,j;
		i=rowPos-1;
		j=colPos;
		if(i>=0){
			if(!isCoinInThisLocation(i,j)){
				addPosIntoMovableLcations(i,j);
			}
		}
		if(rowPos==6){
			i=rowPos-2;
			if(i>=0){
				if(!isCoinInThisLocation(i,j)){
					addPosIntoMovableLcations(i,j);
				}
			}
			
		}
		i=rowPos-1;
		j=colPos-1;
		if(i>=0 && j>=0 && isWhiteCoinInThisLocation(i,j)){
			addPosIntoMovableLcations(i,j);
		}
		i=rowPos-1;
		j=colPos+1;
		if(i>=0 && j<=7 && isWhiteCoinInThisLocation(i,j)){
			addPosIntoMovableLcations(i,j);
		}
	}

	//-----Pawn Helper Methods-------------------------------------------------------------//

	function isCoinInThisLocation(i,j){
		var posStr=concatTwoIntegersToString(i,j);
		if(document.getElementById(posStr).childElementCount==0){
			return false;
		}
		else 
			return true; 
	}
	function isBlackCoinInThisLocation(i,j){
		var posStr=concatTwoIntegersToString(i,j);
		if(document.getElementById(posStr).childElementCount!=0){
			var coin=document.getElementById(posStr).getElementsByTagName("img")[0];
			if(coin.dataset.color=="black")
				return true;
			else
				return false;
		}
		else 
			return false; 
	}
	function isWhiteCoinInThisLocation(i,j){
		var posStr=concatTwoIntegersToString(i,j);
		if(document.getElementById(posStr).childElementCount!=0){
			var coin=document.getElementById(posStr).getElementsByTagName("img")[0];
			if(coin.dataset.color=="white")
				return true;
			else
				return false;
		}
		else 
			return false; 
	}


	//----------3.Knight --------------------------------------------------------------------//

	function knightMovablePositions(coinPosition){
		var rowPos=coinPosition[0];
		var colPos=coinPosition[1];
		var pos;
		var i,j;
		var oEcomb=isOddEvenCombination(rowPos,colPos);
		for(i=rowPos-2;i<=rowPos+2;i++){
			if(i<0 || i==rowPos ) continue;
			if(i>7) break;
			for(j=colPos-2;j<=colPos+2;j++){
				if(j<0 || j==colPos ) continue;
				if(j>7) break;
				if( (   (!oEcomb) && isOddEvenCombination(i,j) ) || (oEcomb&&isOddCombinationOrEvenCombination(i,j))  ){
					if(!(board[i][j]==movingCoin.dataset.color)){
					addPosIntoMovableLcations(i,j);
					}
				}
			}
		}
		
	}

	//----------Knight Helper Methods--------------------------------------------------------------------//
	function isOddEvenCombination(i,j){
		if(isEven(i)&&isOdd(j) || isOdd(i)&&isEven(j)){
			return true;
		}
		else false;
	}
	function isOddCombinationOrEvenCombination(i,j){
		if(isEven(i)&&isEven(j) || isOdd(i)&&isOdd(j)){
			return true;
		}
		else false;
	}
	function isEven(n) {
   		return n % 2 == 0;
	}

	function isOdd(n) {
   		return Math.abs(n % 2) == 1;
	}
	

	//----------4.Bishop--------------------------------------------------------------------//

	function bishopMovablePositions(coinPosition){
		var rowPos=coinPosition[0];
		var colPos=coinPosition[1];
		var pos;
		var i,j;
		for(i=rowPos-1,j=colPos-1; i>=0&&j>=0 ;i--,j--){
			if(board[i][j]==movingCoin.dataset.color) break;
			if(board[i][j]!=movingCoin.dataset.color && board[i][j]!=null ){
				addPosIntoMovableLcations(i,j);
				break;
			}
			addPosIntoMovableLcations(i,j);
		}
		for(i=rowPos-1,j=colPos+1; i>=0&&j<8 ;i--,j++){
			if(board[i][j]==movingCoin.dataset.color) break;
			if(board[i][j]!=movingCoin.dataset.color && board[i][j]!=null ){
				addPosIntoMovableLcations(i,j);
				break;
			}
			addPosIntoMovableLcations(i,j);
		}
		for(i=rowPos+1,j=colPos-1; i<8&&j>=0 ;i++,j--){
			if(board[i][j]==movingCoin.dataset.color) break;
			if(board[i][j]!=movingCoin.dataset.color && board[i][j]!=null ){
				pos=concatTwoIntegersToString(i,j);
				movableLocations.push(pos);
				break;
			}
			addPosIntoMovableLcations(i,j);
		}
		for(i=rowPos+1,j=colPos+1; i<8&&j<8 ;i++,j++){
			if(board[i][j]==movingCoin.dataset.color) break;
			if(board[i][j]!=movingCoin.dataset.color && board[i][j]!=null ){
				addPosIntoMovableLcations(i,j);
				break;
			}
			addPosIntoMovableLcations(i,j);
		}
	}

	//----------5.King--------------------------------------------------------------------//

	function kingMovablePositions(coinPosition){
		var rowPos=coinPosition[0];
		var colPos=coinPosition[1];
		var pos;
		var i,j;
		for(i=rowPos-1;i<=rowPos+1;i++){
			if(i<0) continue;
			if(i>7) break;
			for(j=colPos-1;j<=colPos+1;j++){
				if(j<0) continue;
				if(j>7) break;
				if(!(i==rowPos && j==colPos)){
					if(isCoinInThisLocation(i,j)){
						if(movingCoin.dataset.color=="white"){
							if(isBlackCoinInThisLocation(i,j)){
								addPosIntoMovableLcations(i,j);
							}
						}
						if(movingCoin.dataset.color=="black"){
							if(isWhiteCoinInThisLocation(i,j)){
								addPosIntoMovableLcations(i,j);
							}
						}
					}
					else{
						addPosIntoMovableLcations(i,j);
					}
				}
			}
		}
	}

	//----------1.Rook--------------------------------------------------------------------//

	function rookMovablePositions(coinPosition){
		var rowPos=coinPosition[0];
		var colPos=coinPosition[1];
		var pos;
		var i,j;
		for(i=rowPos,j=colPos+1;j<8;j++){
			if(board[i][j]==movingCoin.dataset.color) break;
			if(board[i][j]!=movingCoin.dataset.color && board[i][j]!=null ){
				addPosIntoMovableLcations(i,j);
				break;
			}
			pos=concatTwoIntegersToString(i,j);
			movableLocations.push(pos);
		}
		for(i=rowPos,j=colPos-1;j>=0;j--){
			if(board[i][j]==movingCoin.dataset.color) break;
			if(board[i][j]!=movingCoin.dataset.color && board[i][j]!=null ){
				addPosIntoMovableLcations(i,j);
				break;
			}
			addPosIntoMovableLcations(i,j);
		}
		for(i=rowPos+1,j=colPos;i<8;i++){
			if(board[i][j]==movingCoin.dataset.color) break;
			if(board[i][j]!=movingCoin.dataset.color && board[i][j]!=null ){
				addPosIntoMovableLcations(i,j);
				break;
			}
			addPosIntoMovableLcations(i,j);
		}
		for(i=rowPos-1,j=colPos;i>=0;i--){
			if(board[i][j]==movingCoin.dataset.color) break;
			if(board[i][j]!=movingCoin.dataset.color && board[i][j]!=null ){
				addPosIntoMovableLcations(i,j);
				break;
			}
			addPosIntoMovableLcations(i,j);
		}
		
	}

	////------------------------------------------End of finding Movable positions--------------------------------------////

	
	function makeMovableLocationClickable(){
		for(var i=0;i<movableLocations.length;i++){
			document.getElementById(movableLocations[i]).onclick=move;
		}
		
		//movingCoin.parentElement.onclick=null;
		
	}

	function makeMovableLocationUnClickable(){
		for(var i=0;i<movableLocations.length;i++){
			document.getElementById(movableLocations[i]).onclick=null;
		}
		
	
		
	}

	function showMovableLocations(){
		var location;
		for(var i=0;i<movableLocations.length;i++){
			location=document.getElementById(movableLocations[i]);
			if(location.childElementCount==0)
				location.style="background-color:Teal";
			else{
				location.style="background-color:red";
				//location.getElementsByTagName("img")[0].width="54px";
				//location.getElementsByTagName("img")[0].height="53px";
			}
			movingCoin.parentElement.style="background-color:Lime";	
			
		}
		
	}

	
	function unShowMovableLocations(){
		var location;
		for(var i=0;i<movableLocations.length;i++){
			location=document.getElementById(movableLocations[i]);
			location.style="background-color:"+location.dataset.color;
		}
		if(movingCoin!=null){
		movingCoin.parentElement.style="background-color:"+movingCoin.parentElement.dataset.color;
		}
	}


	function move(evntObj){
		var clickedCell=evntObj.target;
		if(clickedCell.localName=="img") clickedCell=clickedCell.parentElement;
		var cellBeforeMovement=movingCoin.parentElement;
		cellBeforeMovement.style="background-color:"+cellBeforeMovement.dataset.color;
		if(clickedCell.childElementCount!=0){
			clickedCell.removeChild(clickedCell.getElementsByTagName("img")[0]);
		}
		clickedCell.appendChild(movingCoin);
		updateBoard(cellBeforeMovement.id,clickedCell.id);
		//cellBeforeMovement.removeChild(movingCoin);
		makeMovableLocationUnClickable();
		unShowMovableLocations();
		giveControlToOpponentColor();
		
		
	}
	function giveControlToOpponentColor(){
		var currentColor=movingCoin.dataset.color
		if(currentColor=="white"){
			makeCoinsMovable("black");
			makeCoinsImMovable("white");
		}
		if(currentColor=="black"){
			makeCoinsMovable("white");
			makeCoinsImMovable("black");
		}
		
	}
	function updateBoard(oldPos,newPos){
		var posArr=idStringToIntArray(oldPos);
		var row=posArr[0];
		var col=posArr[1];
		board[row][col]=null;

		posArr=idStringToIntArray(newPos);
		row=posArr[0];
		col=posArr[1];
		board[row][col]=movingCoin.dataset.color;
	}

	function idStringToIntArray(str){
			var intArray=new Array();
			var firstChar=str.charAt(0);
			var secondChar=str.charAt(1);
			var firstDigit=parseInt(firstChar);
			var secondDigit=parseInt(secondChar);
			intArray.push(firstDigit);
			intArray.push(secondDigit);
			return(intArray);
	}
	function concatTwoIntegersToString(i,j){
			var str=i.toString()+j.toString();
			return str;
	}
	
	
</script>

</head>
<body style="background-color:pink">
	<h1 align="center">Let's play</h1>
	
	<table align="center">
		<tr >
			<td id="00"style="background-color:black" data-color="black" width="57px" height="56px"> <img width="54px" height="53px"   name="rook"   data-color="white" 
			class="white" src="whiteCoins/rook.png"> </td>

			<td id="01"style="background-color:white" data-color="white"> <img width="54px" height="53px" name="knight" data-color="white" 
			class="white" src="whiteCoins/knight.png"></td>

			<td id="02"style="background-color:black" data-color="black"> <img width="54px" height="53px" name="bishop" data-color="white" 
			class="white" src="whiteCoins/bishop.png"></td>
			<td id="03"style="background-color:white" data-color="white"> <img width="54px" height="53px" name="queen"  data-color="white" 
			class="white" src="whiteCoins/queen.png"> </td>

			<td id="04"style="background-color:black" data-color="black"> <img width="54px" height="53px" name="king"   data-color="white" 
			class="white" src="whiteCoins/king.png">  </td>

			<td id="05"style="background-color:black" data-color="white"> <img width="54px" height="53px" name="bishop" data-color="white" 
			class="white" src="whiteCoins/bishop.png"></td>

			<td id="06"style="background-color:white" data-color="black"> <img width="54px" height="53px" name="knight" data-color="white" 
			class="white" src="whiteCoins/knight.png"></td>

			<td id="07"style="background-color:black" data-color="white"> <img width="54px" height="53px" name="rook"  data-color="white" 
			class="white" src="whiteCoins/rook.png">  </td>
		</tr>
		<tr>
			<td id="10"style="background-color:white" data-color="white"> <img width="54px" height="53px" name="pawn"  
			data-color="white" class="white" src="whiteCoins/pawn.png"> </td>

			<td id="11"style="background-color:black" data-color="black" width="57px" height="56px"> <img width="54px" height="53px" 
			name="pawn"   data-color="white" class="white" src="whiteCoins/pawn.png"></td>

			<td id="12"style="background-color:white" data-color="white"> <img width="54px" height="53px" name="pawn"
			data-color="white" class="white" src="whiteCoins/pawn.png"></td>

			<td id="13"style="background-color:black" data-color="black"> <img width="54px" height="53px" name="pawn" data-color="white"  
                        class="white" src="whiteCoins/pawn.png">
			</td>

			<td id="14"style="background-color:white" data-color="white"> <img width="54px" height="53px" name="pawn" data-color="white"  
			class="white" src="whiteCoins/pawn.png">
			</td>

			<td id="15"style="background-color:black" data-color="black"> <img width="54px" height="53px" name="pawn" data-color="white" 
 			class="white" src="whiteCoins/pawn.png">
			</td>

			<td id="16"style="background-color:white" data-color="white"> <img width="54px" height="53px" name="pawn" data-color="white" 
     			class="white" src="whiteCoins/pawn.png">
			</td>

			<td id="17"style="background-color:black" data-color="black"> <img width="54px" height="53px" name="pawn" data-color="white"  
			class="white" src="whiteCoins/pawn.png">
			</td>
		</tr>
		<tr>
			<td id="20"style="background-color:black" data-color="black"></td>
			<td id="21"style="background-color:white" data-color="white"></td>
			<td id="22"style="background-color:black" data-color="black" width="57px" height="56px"></td>
			<td id="23"style="background-color:white" data-color="white"></td>
			<td id="24"style="background-color:black" data-color="black"></td>
			<td id="25"style="background-color:white" data-color="white"></td>
			<td id="26"style="background-color:black" data-color="black"></td>
			<td id="27"style="background-color:white" data-color="white"></td>
		</tr>
		<tr>
			<td id="30"style="background-color:white" data-color="white"></td>
			<td id="31"style="background-color:black" data-color="black" ></td>
			<td id="32"style="background-color:white" data-color="white"></td>
			<td id="33"style="background-color:black" data-color="black" width="57px" height="56px"></td>
			<td id="34"style="background-color:white" data-color="white"></td>
			<td id="35"style="background-color:black" data-color="black"></td>
			<td id="36"style="background-color:white" data-color="white"></td>
			<td id="37"style="background-color:black" data-color="black"></td>
		</tr>
		<tr>
			<td id="40"style="background-color:black" data-color="black"></td>
			<td id="41"style="background-color:white" data-color="white"></td>
			<td id="42"style="background-color:black" data-color="black"></td>
			<td id="43"style="background-color:white" data-color="white"></td>
			<td id="44"style="background-color:black" data-color="black" width="57px" height="56px"></td>
			<td id="45"style="background-color:white" data-color="white"></td>
			<td id="46"style="background-color:black" data-color="black"></td>
			<td id="47"style="background-color:white" data-color="white"></td>
		</tr>
		<tr>
			<td id="50"style="background-color:white" data-color="white"></td>
			<td id="51"style="background-color:black" data-color="black"></td>
			<td id="52"style="background-color:white" data-color="white"></td>
			<td id="53"style="background-color:black" data-color="black"></td>
			<td id="54"style="background-color:white" data-color="white"></td>
			<td id="55"style="background-color:black" data-color="black" width="57px" height="56px"></td>
			<td id="56"style="background-color:white" data-color="white"></td>
			<td id="57"style="background-color:black" data-color="black"></td>
		</tr>
		<tr>
			<td id="60"style="background-color:black" data-color="black"><img width="54px" height="53px" name="pawn" data-color="black" 
			class="black" src="BlackCoins/pawn.png"></td>

			<td id="61"style="background-color:white" data-color="white"><img width="54px" height="53px" name="pawn" data-color="black" 
			class="black" src="BlackCoins/pawn.png"></td>

			<td id="62"style="background-color:black" data-color="black"><img width="54px" height="53px" name="pawn" data-color="black" 
			class="black" src="BlackCoins/pawn.png"></td>

			<td id="63"style="background-color:white" data-color="white"><img width="54px" height="53px" name="pawn" data-color="black" 
			class="black" src="BlackCoins/pawn.png"></td>

			<td id="64"style="background-color:black" data-color="black"><img width="54px" height="53px" name="pawn" data-color="black" 
			class="black" src="BlackCoins/pawn.png"></td>

			<td id="65"style="background-color:white" data-color="white"><img width="54px" height="53px" name="pawn" data-color="black" 
			class="black" src="BlackCoins/pawn.png"></td>

			<td id="66"style="background-color:black" data-color="black" width="57px" height="56px"><img width="54px" height="53px" 
			name="pawn" data-color="black" class="black" src="BlackCoins/pawn.png"></td>

			<td id="67"style="background-color:white" data-color="white"><img width="54px" height="53px" name="pawn" data-color="black" 
			class="black" src="BlackCoins/pawn.png"></td>
		</tr>
		<tr>
			<td id="70"style="background-color:white" data-color="white"><img width="54px" height="53px" name="rook"   data-color="black" 
			class="black" src="BlackCoins/rook.png"  ></td>

			<td id="71"style="background-color:black" data-color="black"><img width="54px" height="53px" name="knight" data-color="black" 
			class="black" src="BlackCoins/knight.png"> </td>

			<td id="72"style="background-color:white" data-color="white"><img width="54px" height="53px" name="bishop" data-color="black" 
			class="black" src="BlackCoins/bishop.png"> </td>

			<td id="73"style="background-color:black" data-color="black"><img width="54px" height="53px" name="queen"  data-color="black" 
			class="black" src="BlackCoins/queen.png" > </td>

			<td id="74"style="background-color:white" data-color="white"><img width="54px" height="53px" name="king"   data-color="black" 
			class="black" src="BlackCoins/king.png"  > </td>
			<td id="75"style="background-color:black" data-color="black"><img width="54px" height="53px" name="bishop" data-color="black" 
			class="black" src="BlackCoins/bishop.png"> </td>

			<td id="76"style="background-color:white" data-color="white"><img width="54px" height="53px" name="knight" data-color="black" 
			class="black" src="BlackCoins/knight.png"> </td>

			<td id="77"style="background-color:black" data-color="black" width="57px" height="56px"><img width="54px" height="53px"  
                        name="rook"  data-color="black" class="black" src="BlackCoins/rook.png"  > </td>
		</tr>
		
	</table>
</body>
</html>
