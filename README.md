# DiffTS

Diff for Typescript.

Japanese: [README_JA.md](README_JA.md)

# How to use.

## Browser

Use `dest/browser/diffts.js` .

Sample: https://hirokimiyaoka.github.io/DiffTS/

```
<script type="text/javascript" src="./diffts.js"></script>
<script type="text/javascript">
function diff( a, b ) {
	var d = new DiffTS.Diff();
	return d.diff( new DiffTS.StringLines( a ),new DiffTS.StringLines( b ) );
}
function clear(){
	var e = document.getElementById( 'result' );
	for ( var i = e.children.length - 1 ; 0 <= i ; --i ) { e.removeChild( e.children[ i ] ); }
	return e;
}
function start() {
	var name = [ 'delete', 'equal', 'insert' ];
	var d = diff( document.getElementById( 'a' ).value, document.getElementById( 'b' ).value );
	var e = clear();
	d.forEach( function( item ) {
		var cname = name[ item.type + 1 ];
		item.data.getAll().forEach( function( line ) {
			var li = document.createElement( 'li' );
			li.classList.add( cname );
			li.textContent = line;
			e.appendChild( li );
		} );
	} );
}
document.addEventListener('DOMContentLoaded',function() {
	document.getElementById( 'start' ).addEventListener( 'click', start,false );
}, false );
</script>
```

## Node.js

Use `dest/nodejs/diff.ts` .

```
const diffts = require( './dest/nodejs/diffts' );

function print( diffs )
{
	const name = [ '-', '=', '+' ];
	diffs.forEach( function( item )
	{
		console.log( name[ item.type + 1 ], item.data.getAll() );
	} );
}

console.log( diffts );

const d = new diffts.Diff();

const a = d.diff( new diffts.StringLines( 'a\nb\nc' ), new diffts.StringLines( 'a\nB\nc' ) );
print( a );
```

# Other Diff

Extends `DiffTS.DiffItem` .

Sample: `DiffTS.StringLines`

```
	class StringLines extends DiffTS.DiffItem
	{
		private lines: string[];

		constructor( texts: string | string[] )
		{
			super();
			if ( typeof texts === 'string' )
			{
				this.lines = texts.split( /\r\n|\r|\n/ );
			} else
			{
				this.lines = texts;
			}
		}

		public size() { return this.lines.length; }

		public subItem( start: number, end?: number )
		{
			if ( end === undefined ) { end = this.size(); }
			const lines: string[] = [];
			for ( ; start < end ; ++start ) { lines.push( this.lines[ start ] ); }
			return new StringLines( lines );
		}

		public get( index: number ) { return this.lines[ index ]; }
		public getAll() { return this.lines; }

		public indexOf( value: any )
		{
			for ( let i = 0 ; i < this.size() ; ++i )
			{
				if ( this.lines[ i ] === value ) { return i; }
			}
			return -1;
		}
	}
```

# How to build

Installed TypeScript.

```
npm run build
```

Output in `dest/` .

