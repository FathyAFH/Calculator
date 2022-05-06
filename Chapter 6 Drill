#include "std_lib_facilities.h"

double expression();

class Token   // c is missing - This is the class token that is a user-defined data type that holds the kind and value types
{
public:
    char kind; //kind is for the operators such as +,-,*,/
    double value; //value to indicate the numeric values
    Token (char ch): kind(ch), value(0) {} //we are creating the token from a character
    Token (char ch, double val): kind(ch), value(val) {} //we create a token from both the character and the value
};

class Token_stream
{
public:
    Token_stream(); // the token stream will read from the values inputted by the user
    Token get();//get the token is storing the inputted token
    void putback(Token t); //put the token back
private:
    bool full; // it checks if the token is in the buffer or not, if yes, it will be in the buffer (a temporary storage)
    Token buffer;
};

Token_stream::Token_stream(): full(false), buffer(0) {} //if sets full to a false value if the buffer is empty

void Token_stream::putback(Token t) //the putback function basically puts the argument back to the buffer of the token_stream
{
    if (full) error("Token_stream buffer full");
    buffer = t;
    full = true;
}

Token Token_stream::get()
{
    if (full) //do we have a token already?
    {
        full = false; //in this case, the buffer will be removed
        return buffer;
    }

    char ch;
    cin >> ch; //inputting characters (skips space, new tab, newline, etc.)

    switch(ch)
    {
    case 'x': // q is changed to x
    case '=': // ; is changed  to =
    case '(':
    case ')':
    case '+':
    case '-':
    case '*':
    case '/':
    case '%': //add the remainder case
        return Token(ch); //let each character represents itself
    case '.':
    case '0':
    case '1':
    case '2':
    case '3':
    case '4':
    case '5':
    case '6':
    case '7':
    case '8': //adding case 8 as it was missing
    case '9':
    {
        cin.putback(ch); //put the digit back into the input stream
        double val = 0;
        cin >> val; //assigning a double value that will be read from the user input
        return Token('8', val); //let 8 represent a numeric value for the token
    }
    default:
        error("Bad token");
        return 0;
    }
}

Token_stream ts; //provides get() and putback()

double primary()
{
    Token t = ts.get();
    switch(t.kind)
    {
    case '(': //for the case of brackets
    {
        double d = expression();
        t = ts.get();
        if (t.kind != ')') error("')' expected"); // " was missing and if there was a missing bracket, an error message should be displayed
        return d;
    }
    case '8':
        return t.value; //returns the number's value
    default:
        error("primary expected");
        return 0;
    }
}

double term()
{
    double left = primary(); //assigning the left value to be a primary value
    Token t = ts.get(); //get the next token from token stream
    while(true)
    {
        switch(t.kind)
        {
        case '*':
            left *= primary();
            t = ts.get();
            break;
        case '/':
        {
            double d = primary();
            if ( d == 0 ) error("divide by zero");
            left /= d;
            t = ts.get();
            break;
        }

        case '%':
        {
            double d = primary();
            if (d == 0) error("ERROR: Cannot divide by zero !");
            left = remainder(left,d);
            t = ts.get();
            break;
        }
        default:
            ts.putback(t); //put token back to the token stream
            return left;
        }
    }
}
// this is used to deal between + and -
double expression()
{
    double left = term(); //read and evaluate a term
    Token t = ts.get(); //get the next token form the token stream
    while(true)
    {
        switch(t.kind)
        {
        case '+':
            left += term(); //evaluate the term and add
            t = ts.get();
            break;
        case '-':
            left -= term(); //evaluate the term and subtract
            t = ts.get();
            break;
        default:
            ts.putback(t); //put t back to the token stream
            return left; //return the answer if the there is no - or +
        }
    }
}

int main()
try
{

    double val = 0;
    cout << "Welcome to our simple calculator.\n"
         << "Please enter expressions using floating-point numbers.\n" //Add a greeting line in main()/
         << "You can use the following expressions :\n" << endl << "+ to add\n" << "- to subtract\n" << "* to multiply\n" << "/ to divide\n" << "% for the remainder\n" << endl
         << "End your expression with an '=' to print, press 'x' to quit.\n"; // Improve that greeting writing instructions for the operators and how to output or end code./


    while(cin)
    {
        Token t = ts.get();

        if (t.kind == 'x') break; // q is changed to x
        if (t.kind == '=') //; is changed to =
            cout << "=" << val << endl;
        else
            ts.putback(t);

        val = expression();
    }

    return 0;

}
catch (exception& e)
{
    cerr << "Error: " << e.what() << endl;
    return 1;
}
catch (...)
{
    cerr << "exception\n";
    return 2;
}
