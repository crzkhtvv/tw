  function translate(text, dictionary){
	if (typeof text !== 'string'){
		throw new Error('TypeError')
	}
	if (typeof dictionary !== 'object' || !dictionary){
		throw new Error('TypeError')
	}
	for (let prop in dictionary){
		if (typeof dictionary[prop] !== 'string'){
			throw new Error('TypeError')
		}
	}
	let result = text.split(' ')
	for (let prop in dictionary){
		let position = result.indexOf(prop)
		if (position !== -1){
			result[position] = dictionary[prop]
		}	
	}
	return result.join(' ')
}
-----js



app.get('/create', async (req, res) => {
	try{
		await sequelize.sync({force : true})
		for (let i = 0; i < 10; i++){
			let author = new Author({
				name : 'name ' + i,
				email : 'name' + i + '@nowhere.com',
				address : 'some address on ' + i + 'th street'
			})
			await author.save()
		}
		res.status(201).json({message : 'created'})
	}
	catch(err){
		console.warn(err.stack)
		res.status(500).json({message : 'server error'})
	}
})

app.get('/authors', async (req, res) => {
	// should get all authors
	try{
		let authors = await Author.findAll()
		res.status(200).json(authors)
	}
	catch(err){
		console.warn(err.stack)
		res.status(500).json({message : 'server error'})		
	}
})

app.post('/authors', async (req, res) => {
	try{
		let author = new Author(req.body)
		await author.save()
		res.status(201).json({message : 'created'})
	}
	catch(err){
		// console.warn(err.stack)
		res.status(500).json({message : 'server error'})		
	}
})

app.post('/authors/:id/books', async (req, res) => {
	// should add a book to an author
	try{
		let author = await Author.findById(req.params.id)
		if (author){
			let book = req.body
			book.authorId = author.id
			await Book.create(book)
			res.status(201).json({message : 'created'})
		}
		else{
			res.status(404).json({message : 'not found'})
		}
	}
	catch(e){
		console.warn(e)
		res.status(500).json({message : 'server error'})
	}
})

app.get('/authors/:id/books', async (req, res) => {
	// should list all of an authors' books
	try{
		let author = await Author.findById(req.params.id, {include : [Book]})
		if (author){
			let books = author.books
			res.status(200).json(books)
		}
		else{
			res.status(404).json({message : 'not found'})
		}
	}
	catch(e){
		console.warn(e)
		res.status(500).json({message : 'server error'})
	}
})
---rest


{
    "name": "Some Name",
    "year": 3,
    "credits": 100,
    "superpowers": ["a", "b", "c"],
    "grades": [{
        "class": "Some class",
        "homeworkSubmitted": true,
        "examGrade": 5
    },{
        "class": "Some class",
        "homeworkSubmitted": false,
        "examGrade": 5
    }]
}
---json


*{
	box-sizing: border-box;
	border : 1px solid black;
	margin : 0px;
	padding: 0px;
	font-size: xx-large;
}

.main{
	display : grid;
	width: 80vw;
	height: 80vh;
	max-width:100%;
	max-height:100%;
	grid-template-columns: 20vw 60vw;
	grid-template-rows: 80vh;
	grid-template-areas: 
		"sidebar view"
		;
}

.sidebar{
	grid-area : sidebar;
}

.sidebar-item{
	height : 20vh;
}	

.view{
	grid-area : view;
}

.top{
	height : 20vh;
}

.bottom{
	height : 60vh;
}
---css

<!DOCTYPE html>
<html>
    <head>
        
    </head>
    <body>
        <p>Salut</p>
       <div id="obiect">
           
       </div>
       
        <script>
            const res = fetch('invoice.json')
            .then(res => res.json())
            .then(data => {
                console.log(data);
                document.getElementById("obiect").innerHTML=`<div>${data.invoiceSeries}</div>`
            });



            
        </script>
    </body>
</html>
---html



