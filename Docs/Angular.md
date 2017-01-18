$scope.$watch('myVar', function(newVal, oldVal){
	// Instructions ...
})

forcer la mise à jour (lancement du cycle digest)
$scope.$apply(function(){
	// Instructions ...
})

ng-cloak: eviter que le template affhiche la version brute


