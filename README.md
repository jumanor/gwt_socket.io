gwt-socket.io
==============
A simple GWT wrapper for javascript socket.io

Download [TJSocketIO-0.3.jar](https://drive.google.com/open?id=0B72oLqC-8YVbWTZMUVUzYzlNaW8)

Update your .gwt.xml files to include this:

    <inherits name="com.epiko.TJSocketIO"/>

Getting Started
===============
**FIRST EXAMPLE**

Client (HTML)

    <script src="/socket.io/socket.io.js"></script>
    <script>
            var socket = io.connect('http://localhost:9090');
            socket.on('news', function (data) {
                console.log(data);
                socket.emit('my event', data+' new');
            });
    </script>

Client (GWT)    
    
    ScriptInjector.fromUrl("/socket.io/socket.io.js").setCallback(new Callback<Void, Exception>() {

			@Override
			public void onSuccess(Void result ) {
				// TODO Auto-generated method stub
				final TJSocketIO socket=TJSocketIO.connect("http://localhost:9090");
                socket.onSocket("news",new SocketHandler<String>() {

					@Override
					public void onSocket(String data ) {
						// TODO Auto-generated method stub
						GWT.log(data);
						socket.emitSocket("my event",data+" new");
					}
				});
			}

			@Override
			public void onFailure(Exception reason) {
				// TODO Auto-generated method stub
				
			}
			
	}).inject();
 
**SECOND EXAMPLE**

Client (HTML)

    <script>
            socket.on('connect', function () {
               
                socket.emit('my event','hello jumanor',function(data){
                    
                    console.log(data);
                    
                });
            });
    </script>

Client (GWT)    

    socket.onSocket("connect", new SocketHandler<Void>() {

		@Override
		public void onSocket(Void data) {
		// TODO Auto-generated method stub
						
			socket.emitSocketResponse("my event", "hello jumanor", new SocketHandler<String>() {

                @Override
                public void onSocket(String data) {
                    // TODO Auto-generated method stub
                    GWT.log(data);                    
                }
							
			});
	    }
					
	});
    
**THIRD EXAMPLE**

Client (HTML)

    <script>
            socket.on('my event', function (data,response) {
               
                var resp={};
                resp.data=data;
                
                response(resp);
            });
    </script>

Client (GWT)

    socket.onSocket("my event",new SocketAndResponseHandler<String,JavaScriptObject>() {

		@Override
		public void onSocketAndResponse(String data,ResponseHandler<JavaScriptObject> respuesta) {
			// TODO Auto-generated method stub
						
			JSONObject resp=new JSONObject();
			resp.put("data", new JSONString(data));
						
			respuesta.onResponse(resp.getJavaScriptObject());
						
		}
    });
