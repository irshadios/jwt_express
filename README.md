# jwt_express


### Install 
  sudo npm install express jsonwebtoken
  
### main.js
    
    const express = require('express')
    const jwt = require('jsonwebtoken')

    const app = express()

    app.get('/api', (req, res) => {
      res.json({
        message: "Welcome to the api"
      })
    })

    app.post('/api/posts', verifyToken, (req, res) => {
      jwt.verify(req.token, 'secretkey', (err, authData) => {
          if(err) res.sendStatus(403)

          res.json({
              message: "Post created....",
              authData: authData,

          })
      })
    })


      app.post('/api/login', (req, res) => {
          // Mock user
          const user = {
              id: 1,
              username: 'vishnuguputa',
              email: 'vishnugupta@gmail.com'
          }
          jwt.sign({user: user}, 'secretkey',  { expiresIn : '30s' } , (err, token) => {
              res.json({
                  token: token
              })
          })
      })
      // FORMAT OF TOKEN
      // Authorization: Bearer <access_token>


      // verify token


      function verifyToken(req, res, next){
          // Get auth header value
          const bearerHeader = req.headers["authorization"];

          // Check if bearer is undefined
          if(typeof bearerHeader !== 'undefined'){

      // Split at the space

      const bearer = bearerHeader.split(' ')
      // get token from array

      const bearerToken = bearer[1]
      // set the token
      req.token = bearerToken
      // Next middleware
      next()

          }else {
              // Forbidden
              res.sendStatus(403)
          }
      }

      app.listen(5000, () => console.log("Server started on port 5000"))
