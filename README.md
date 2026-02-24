# Cliente react

```js
const WS_URL = "http://localhost:8080/ws";

function App() {
  const [turn, setTurn] = useState(null);
  const [ticketCalled, setTicketCalled] = useState(null);
  const [stompClient, setStompClient] = useState(null);

  useEffect(() => {
    // Conectar a WebSocket STOMP
    const socket = new SockJS(WS_URL);
    const client = over(socket);

    client.connect({}, () => {
      client.subscribe("/topic/ticket-called", (msg) => {
        const data = JSON.parse(msg.body);
        setTicketCalled(data);
      });
    });

    setStompClient(client);

    // Cleanup al desmontar
    return () => {
      if (client && client.connected) client.disconnect();
    };
  }, []);

  const getTicket = async () => {
    const res = await fetch("http://localhost:8080/api/turn/ticket");
    if (res.ok) {
      const json = await res.json();
      setTurn(json);
    }
  };

  const checkTicket = async () => {
    const res = await fetch("http://localhost:8080/api/turn/check/ticket");
    if (res.ok) {
      const json = await res.json();
      setTurn(json);
    }
  };

  return (
    <div>
      <h2>Cliente de Turnos</h2>
      <button onClick={getTicket}>Solicitar Turno</button>
      <button onClick={checkTicket} style={{ marginLeft: 10 }}>
        Verificar Turno
      </button>
      <div style={{ marginTop: 20 }}>
        <strong>Turno:</strong>
        <div>
          {turn `ID: ${turn.id}, Estado: ${turn.status}`}
        </div>
      </div>
      <div style={{ marginTop: 20 }}>
        <strong>Ultimo turno llamado en tiempo real:</strong>
        <div>
          {turn `ID: ${turn.id}, Estado: ${turn.status}`}
        </div>
      </div>
    </div>
  );
}

export default App;
```
Este cliente en react:
- Hace peticiones al API de turnos
- Muestra el turno generado
- Permite marcar un turno
- Muestra el tiempo real el ultimo turno llamado
