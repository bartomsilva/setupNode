# setupNode
setup do nodeJs com e sem ts


= start do projeto
```bash
npm init --y
```

= package.json
```bash (scripts)
  "scripts": {
    "start": "node ./build/index.js",
    "build": "tsc",
    "dev": "ts-node-dev ./src/index.ts",
    "test":"jest"
  },
```

= typescript
```bash
npm install -D typescript
npm install -D @types/node
npx tsc --init
```

= tsconfig.json
```bash
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
  },
  "exclude": [
    "node_modules",
    "build",  
    "tests",
    "jest.config.ts"
  ]
}
```
= express
```bash
npm install express
npm install @types/express -D
```

= cors
```bash
npm install cors
npm install @types/cors -D
```

= ts-node-dev
```bash
npm install ts-node-dev -D
```

= dotenv
```bash
npm install dotenv
```

= bcrypt
```bash
npm i bcryptjs
npm i -D @types/bcryptjs
```

= bcripty class

```bash
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



= jwtoken
```bash
npm install jsonwebtoken
npm install -D @types/jsonwebtoken
```
= exemplo de payload
```bash
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


= setup basico
```bash
index.js / app.js / etc
import  express, { Request, Response} from 'express'
import cors from 'cors';

dotenv.config()

const app = express();
app.use(express.json());
app.use(cors());

```








