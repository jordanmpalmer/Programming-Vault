website: docs.GL
## Vertex buffer

```cpp

// create values for buffer
float positions[6] =
{
	-0.5f,-0.5f,
	 0.0f, 0.5f,
	 0.5f,-0.5f,
};

// generate buffer
unsigned int buffer;        // this is GLuint
glGenBuffers( 1, &buffer);

// select the buffer
glBindBuffer( GL_ARRAY_BUFFER, buffer );

// copy data into buffer
glBufferData( GL_ARRAY_BUFFER, 6 * sizeof(float), positions, GL_STATIC_DRAW );


while ( !glfwWindowShouldClose( window ) )
{
	glClear( GL_COLOR_BUFFER_BIT );

	// draw currently selected(bound) buffer
	glDrawArrays( GL_TRIANGLES, 0, 3);

	glfwSwapBuffers( window );

	glfwPollEvents();
}
```

## Vertex Attributes 

```cpp
// Envables generic vertex attibute array
glEnableVertexAttribArray( 0 );
// create attirbute layout
glVertexAttribPointer( 0, 2, GL_FLOAT, GL_FALSE, 2 * sizeof(float), 0);

void glVertexAttribPointer(	index, size, type, normalized, stride, *pointer);

```

## Shaders

```cpp
static unsigned int CompileShader( unsigned int type, const std::string& source )
{
    unsigned int id = glCreateShader( type );
    const char* src = source.c_str();
    glShaderSource( id, 1, &src, nullptr );
    glCompileShader( id );
    
    // Error handling
    int result;
    glGetShaderiv( id, GL_COMPILE_STATUS, &result );
    if ( result == GL_FALSE )
    {
        int length;
        glGetShaderiv( id, GL_INFO_LOG_LENGTH, &length );
        char* message = (char*)alloca(length * sizeof(char));
        glGetShaderInfoLog( id, length, &length, message );
        std::cout << "Failed to compile " << (type == GL_VERTEX_SHADER ? "vertex" : "fragment") <<  " shader!" << std::endl;
        std::cout << message << std::endl;
        glDeleteShader( id );
        return 0;
    }
    
    return id;
}

static unsigned int CreateShader( const std::string& vertexShader, const std::string& fragmentShader )
{
    unsigned int program = glCreateProgram();
    unsigned int vs = CompileShader( GL_VERTEX_SHADER, vertexShader );
    unsigned int fs = CompileShader( GL_FRAGMENT_SHADER, fragmentShader );
    
    glAttachShader( program, vs );
    glAttachShader( program, fs );
    glLinkProgram( program );
    glValidateProgram( program );
    
    glDeleteShader( vs );
    glDeleteShader( fs );
    
    return program;
}

int main() 
{
    const std::string vertexShader = R"(
        #version 120

        attribute vec4 position;

        void main()
        {
           gl_Position = position;
        }
        )";
    
    std::string fragmentShader = R"(
        #version 120

        void main()
        {
            gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0);
        }
        )";
    
    unsigned int shader = CreateShader( vertexShader, fragmentShader );
    glUseProgram( shader );
}
```