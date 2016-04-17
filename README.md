gwt-socket.io
==============
A simple GWT wrapper for javascript socket.io

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

    TJSocketIO.getSingleton().loadScript("/socket.io/socket.io.js",new Callback<Void, Exception>() {
			
			@Override
			public void onSuccess(Void result) {
				// TODO Auto-generated method stub
				TJSocketIO.getSingleton().connect("http://localhost:9090");
                TJSocketIO.getSingleton().onSocket("news",new SocketHandler<String>() {

					@Override
					public void onSocket(String data) {
						// TODO Auto-generated method stub
						GWT.log(data);
						TJSocketIO.getSingleton().emitSocket("my event",data+" new");
					}
				});
			}
			
			@Override
			public void onFailure(Exception reason) {
				// TODO Auto-generated method stub
				
			}
	});

**SECOND EXAMPLE**

Client (HTML)

    <script>
            socket.on('connected', function () {
               
                socket.emit('my event','hello jumanor',function(data){
                    
                    console.log(data);
                    
                });
            });
    </script>

Client (GWT)    

    TJSocketIO.getSingleton().onSocket("connected", new SocketHandler<Void>() {

		@Override
		public void onSocket(Void data) {
		// TODO Auto-generated method stub
						
			TJSocketIO.getSingleton().emitSocketResponse("my event", "hello jumanor", new SocketHandler<String>() {

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

    TJSocketIO.getSingleton().onSocket("my event",new SocketAndResponseHandler<String,JavaScriptObject>() {

		@Override
		public void onSocketAndResponse(String data,ResponseHandler<JavaScriptObject> respuesta) {
			// TODO Auto-generated method stub
						
			JSONObject resp=new JSONObject();
			resp.put("data", new JSONString(data));
						
			respuesta.onResponse(resp.getJavaScriptObject());
						
		}
    });