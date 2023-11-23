# setupNode
setup do nodeJs com e sem ts


### start do projeto
```bash
npm init --y
```

### package.json
```bash (scripts)
  "scripts": {
    "start": "node ./build/index.js",
    "build": "tsc",
    "dev": "ts-node-dev ./src/index.ts",
    "test":"jest"
  },
```

### typescript
```bash
npm install -D typescript
npm install -D @types/node
npx tsc --init
```

### tsconfig.json
```json
{
  "compilerOptions": {
    "target": "ES6",                                   
    "module": "commonjs",                                 
    "rootDir": "./src",                                  
    "outDir": "./build",                                
    "removeComments": true,                            
    "esModuleInterop": true,                              
    "forceConsistentCasingInFileNames": true,            
    "noImplicitAny": true,                             
    "strictFunctionTypes": true,                    
    "skipLibCheck": true,                          
    "allowJs": true,
    "strict": true,
    "experimentalDecorators": true,
  },
  "exclude": [
    "node_modules", "build",  "tests", "jest.config.ts" ]
}
```

### express
```bash
npm install express
npm install @types/express -D
```

### cors
```bash
npm install cors
npm install @types/cors -D
```

### ts-node-dev
```bash
npm install ts-node-dev -D
```

### dotenv
```bash
npm install dotenv
```

### mongoose
```bash
npm install mongoose @types/mongoose
``` 

### bcrypt
```bash
npm i bcryptjs
npm i -D @types/bcryptjs
```

### bcripty class
```ts
.env
BCRYPT_COST=12

export class HashManager {
    public hash = async (
			plaintext: string
		): Promise<string> => {
        const rounds = Number(process.env.BCRYPT_COST)
        const salt = await bcrypt.genSalt(rounds)
        const hash = await bcrypt.hash(plaintext, salt)

        return hash
    }
    public compare = async (
	plaintext: string,
	hash: string
	): Promise<boolean> => {
	// aqui não precisa do await porque o return já se comporta como um
        return bcrypt.compare(plaintext, hash)
    }
}

// hash gerado a partir da senha do body
const hashedPassword = await this.hashManager.hash(password)
// o password hasheado está no banco de dados
const hashedPassword = userDB.password

// o serviço hashManager analisa o password do body (plaintext) e o hash
const isPasswordCorrect = await this.hashManager.compare(password, hashedPassword)

// validamos o resultado
if (!isPasswordCorrect) {
	throw new BadRequestError("'email' ou 'password' incorretos")
    }
```

### jwtoken
```bash
npm install jsonwebtoken
npm install -D @types/jsonwebtoken
```

### exemplo de payload
```ts
export interface TokenPayload {
    id: string,
		name: string,
    role: USER_ROLES
}
export class TokenManager {

// converte o objeto de dados (payload) para um token string
   public createToken = (payload: TokenPayload): string => {
        const token = jwt.sign(
            payload,
            process.env.JWT_KEY as string,
            {
                expiresIn: process.env.JWT_EXPIRES_IN
            }
        )
        return token
    }

// valida e converte o token string para um objeto de dados (payload)
    public getPayload = (token: string): TokenPayload | null => {
        try {
            const payload = jwt.verify(
                token,
                process.env.JWT_KEY as string
            )

            return payload as TokenPayload
        
	// se a validação falhar, um erro é disparado pelo jsonwebtoken
	// nós pegamos o erro aqui e retornamos null para a Business saber que falhou
	} catch (error) {
            return null
        }
    }
}​

```
### validação com ZOD

```bash
npm install zod
```

### DTO Design Patterns e DTO
```ts
export interface CreateUserInputDTO {
    id: string,
    name: string,
    email: string,
    password: string
}

export interface CreateUserOutputDTO {
    message: string,
    user: {
        id: string,
        name: string,
        email: string,
        createdAt: string
    }
}

export const CreateUserSchema = z.object({
  id: z.string().min(1),
  name: z.string().min(2),
  email: z.string().email(),
  password: z.string().min(4)
}).transform(data => data as CreateUserInputDTO)

try {
// validação do dado pelo zod
   const input = CreateUserSchema.parse({
      id: req.body.id,
      name: req.body.name,
      email: req.body.email,
      password: req.body.password
   })
```


### setup basico
```ts
index.js / app.js / etc
import  express, { Request, Response} from 'express'
import cors from 'cors';
import mongoose from 'mongoose';

dotenv.config()

const app = express();
app.use(express.json());
app.use(cors());

const DB_URL  = `mongodb+srv://${process.env.DB_USER}:${process.env.DB_PASSWORD}@cluster0.vrkqflq.mongodb.net/${process.env.DB_NAME}?retryWrites=true&w=majority`
mongoose
  .connect(DB_URL)
  .then(() => {
    console.log("conexão como banco de dados bem sucedida...");
    app.listen(3000, ()=> console.log("servidor on port 3000"));
  })
  .catch((err) => console.log(err));


```

### roteador

```ts
import express from 'express'
export const userRouter = express.Router() // criação do router de users

const exampleController = new ExampleController()

userRouter.get("/", exampleController.getExamples)
userRouter.post("/", exampleController.createExample)
userRouter.get("/:id/purchases", exampleController.examplePurchases)
userRouter.put("/:id/password", exampleController.examplePassword)```
```


### MongoDb

```ts

const mongoose = require("mongoose")

const personSchema = new mongoose.Schema({
  name: String,
  salary: Number,
  approved: Boolean,
}, { versionKey: false });

const Person = mongoose.model('Person', personSchema);
module.exports = Person;

// verificação de tipo objectid
const { ObjectId } = mongoose.Types;
if (!ObjectId.isValid(idPerson)) {
return res.status(400).json({ error: "ID inválido" });
}

// inserir no banco de dados
const response = await Person.create(newPerson);

// pesquisa
const response = await Person.find({_id: idPerson});
const response = await Person.findOne(idPerson);
const response = await Person.findById(idPerson);
const respnse = await Person.findByIdUpdate(idPerson,updatePerson)

// verificação update
const response = await Person.updateOne(idPerson, updatePerson);
if (response.matchedCount === 0) {
   throw new Error("Usuário não encontrado!");
}
// verificação delete
const response = await Person.deleteOne({ _id: idPerson });
if(response.deletedCount === 0) {
   throw new Error("Usuário não encontrado!");
}
```






